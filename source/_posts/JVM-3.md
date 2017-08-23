title: JVM之虚拟机字节码执行引擎
date: 2016-04-26 20:26:49
categories:
- JVM
tags:
- jvm
- invoke
---

所有JVM的执行引擎都是一致的：
输入的是字节码文件，
处理过程是字节码解析的等效过程，
输出的是执行结果。

<!--more-->

## 运行时栈帧结构

栈帧是用于支持虚拟机进行方法调用和方法执行的数据结构，它是虚拟机运行时数据区中的虚拟机栈的栈元素。

栈帧存储了方法的局部变量表，操作数栈，动态链接和方法返回地址等信息。

每一个方法从调用开始至执行完成的过程，都对应着一个栈帧在虚拟机栈里面从入栈到出栈的过程。

在活动线程中，只有位于栈顶的栈帧才是有效的，称为当前栈帧。
执行引擎运行的所有字节码指令都只针对当前栈帧进行操作。

### 局部变量表

局部变量表是一组 变量值 存储空间，用于存放方法参数和方法内部定义的局部变量。

在Java程序编译为class文件时，就在方法的Code属性的max_locals数据项中确定了该方法所需要分配的局部变量表的最大容量

其容量以变量槽（Variable Slot）为最小单位。
虚拟机规范很有导向性地说到每个Slot都应该能存放一个boolean、byte、char、short、int、float、reference或returnAddress类型的数据。这8种数据类型都可以用32位或更小的物理内存来存放。

64位虚拟机使用对齐和补白的手段让Slot在外观上看起来和32位虚拟机一致

reference类型表示对一个对象实例的引用，虚拟机规范既没有说明它的长度，也没有明确指出这种引用应有怎么样的结构。
但虚拟机实现至少都应当能通过这个引用做到两点：
1.从此引用中直接或间接地查找到对象在Java堆中的数据存放的起始地址索引
2.此引用中直接或间接地查找到对象所属数据类型在方法区中的存储的类型信息

returnAddress很少见了，为jsr，jsr_w，ret服务的。这几条指令之前是用来实现异常处理的，现在已经由异常表代替了

对于64位的数据类型，虚拟机会以高位对齐的方式为其分配两个连续的Slot空间。
因为局部变量表是建立在线程的堆栈上，是线程私有的数据，无论读写两个连续的Slot是否为原子操作，都不会引起数据安全问题。

虚拟机通过索引定位的方式使用局部变量表，索引值的范围是从0开始至局部变量表最大的Slot数量。

第0位索引是this,然后是参数表，最后根据方法体内部定义的变量顺序和作用域分配其余的Slot

局部变量表Slot复用对垃圾收集的影响

如果一个局部变量定义了但没有赋初始值是不能使用的

### 操作数栈
操作数栈=操作栈

同局部变量表一样，操作数栈的最大深度也在编译的时候写入到Code属性的max_stacks数据项中。

在概念模型中，两个栈帧作为虚拟机的元素，是完全相互独立的。
但在大多数虚拟机的实现里都会做一些优化处理，
令两个栈帧出现一部分重叠：让下面栈帧的部分操作数栈与上面栈帧的部分局部变量表重叠在一起
这样在进行方法调用时就可以共用一部分数据，无须进行额外的参数复制传递

### 动态连接

每个栈帧都包含一个指向运行时常量池中该栈帧所属方法的引用，持有这个引用是为了支持方法调用过程中的动态连接。

### 方法返回地址

正常完成出口
异常完成出口

方法退出的过程实际上等同于把当前栈帧出栈，因此退出时可能执行的操作有：
恢复上层方法的局部变量表和操作数栈
把返回值（如果有的话）压入调用者栈帧的操作数栈中
调整PC计数器的值以指向方法调用指令后面的一条指令等

### 附加信息

与调试相关的信息。

在实际开发中，一般会把动态连接、方法返回地址与其他附加信息全部归为一类，称为栈帧信息

## 方法调用

方法调用并不等同于方法执行，方法调用阶段唯一的任务就是确定被调用方法的版本（即调用哪一个方法），暂时还不涉及方法内部的具体运行过程。

### 解析
在类加载的解析阶段，会将其中的一部分符号引用转换为直接引用。
这种解析能成立的前提是：方法在程序真正运行之前就有一个可确定的调用版本，并且这个方法的调用版本在运行期是不可改变的。换句话说，调用目标在程序代码写好、编译器进行编译时就必须确定下来。
这类方法的调用称为解析

符合“编译期可知，运行期不可变”这个要求的方法，主要包括静态方法和私有方法。

invokestatic：调用静态方法
invokespecial：调用实例构造器<init>方法、私有方法和父类方法
invokevirtual：调用所有的虚方法
invokeinterface：调用接口方法，会在运行时再确定一个实现此接口的对象
invokedynamic：先在运行时动态解析出调用点限定符所引用的方法，然后再执行该方法。

前4个分派逻辑是固化在JVM内部的。
invokedynamic指令的分派逻辑是由用户所设定的引导方法决定的。

只要能被invokestatic和invokespecial指令调用的方法，都可以在解析阶段中确定唯一的调用版本，符合这个条件的有静态方法、私有方法、实例构造器、父类方法4类，它们在类加载的时候就会把符号引用解析为该方法的直接引用。这些方法可以称为非虚方法

与之相反，其他方法称为虚方法（final除外）

解析调用一定是一个静态的过程

### 分派

1.静态分派

静态类型与实际类型

虚拟机（确切的说是编译器）在重载时是通过参数的静态类型而不是实际类型作为判断依据的。

静态类型是编译器可知的，因此，在编译阶段，Javac编译器会根据参数的静态类型决定使用哪个重载版本，选择某个方法作为调用目标，并把这个方法的符号引用写到main()方法里的两条invokevirtual指令的参数中

所有依赖静态类型来定位方法执行版本的分派动作称为静态分配。

典型应用：方法重载

编译器虽然能确定出方法的重载版本，但在很多情况下这个重载版本并不是“唯一的”，往往只能确定一个“更加合适的”版本

主要原因是字面量不需要定义。

编译期间选择静态分配目标的过程，是Java语言实现方法重载的本质

2.动态分配

动态分配与多态的体现：重写有着很密切的关联。

invokevirtual指令的运行时解析过程大致分为以下步骤：
1.找到操作数栈顶的第一个元素所指向的对象的实际类型，记作C
2.如果在类型C中找到与常量中的描述符和简单名称都相符的方法，则进行访问权限校验，如果通过则返回这个方法的直接引用，查找过程结束。
3.否则，按照继承关系从下往上依次对C的各个父类进行第二步的搜索和验证过程。
4.如果始终没有找到合适的方法，则抛出java.lang.AbstractMethodError异常

由于invokevirtual指令执行的第一步就是在运行期确定接收者的实际类型，所以两次调用中的invokevirtual指令把常量池中的类方法符号引用解析到了不同的直接引用上，这个过程就是Java语言方法重写的本质。


3.单分派与多分派
方法的接收者与方法的参数统称为方法的宗量

根据分派基于多少种宗量，可以将分派划分为单分派和多分派两种。

单分派是根据一个宗量对目标方法进行选择
多分派是根据多于一个宗量对目标方法进行选择


4.虚拟机动态分配的实现

稳定优化手段：

为类在方法区中建立一个虚方法表（Virtual Method Table，也称为vtable，与此对应的，在invokeinterface执行时也会用到接口方法表-Interface Method Table，简称itable）

使用虚方法表索引来代替元数据查找以提高性能。

虚方法表存放着各个方法的实际入口地址

方法表一般在类加载的连接阶段进行初始化，准备了类的变量初始值后，虚拟机会把该类的方法表也初始化完毕。

在条件允许的情况下，还会使用内联缓存（Inline Cache）和基于“类型继承关系分析”（Class Hierarchy Analysis，CHA）技术的守护内联（Guarded Inlining）两种非稳定的“激进优化”手段来获得更高的性能


## 动态类型语言支持

什么是动态类型语言？
动态类型语言的关键特征是它的类型检查的主体过程时在运行期而不是编译期。

“变量无类型而变量值才有类型”这个特点也是动态类型语言的一个重要特征

JDK1.7与动态类型

java.lang.invoke包

MethodHandle可以想象成一个函数指针

从本质上讲，Reflection和MehtodHandle机制都是在模仿方法调用，
但Reflection是在模拟Java代码层次的方法调用，而MethodHandle是在模拟字节码层次的方法调用
Reflection是重量级，而MethodHandle是轻量级

每一处含有invokedynamic指令的位置都称做“动态调用点”（Dynamic Call Site），这条指令的第一个参数不再是代表方法符号引用的CONSTANT_Methodref_info常量，而是变为JDK1.7新加入的CONSTANT_InvokeDynamic_info常量，从这个常量中可以得到3项信息：
引导方法（Bootstrap Method，此方法存放在新增的BootstrapMethods属性中）、方法类型（MethodType）和名称

引导方法是有固定的参数，并且返回值是java.lang.invoke.CallSite对象，这个代表正在要执行的目标方法调用。

根据CONSTANT_InvokeDynamic_info常量中提供的信息，虚拟机可以找到并且执行引导方法，从而获得一个CallSite对象，最终调用要执行的目标方法。

不是很太明白invokedynamic指令演示

由于invokedynamicj指令所面向的使用者并非Java语言，而是其他JVM之上的动态语言，因此仅依靠Java语言的编译器Javac没有办法生成带有invokedynamic指令的字节码。

## 基于栈的字节码解释执行引擎

1.解释执行
Java语言中，Javac编译器完成了程序代码经过词法分析，语法分析到抽象语法树，再遍历语法树生成线性字节码指令流的过程。
因为这一部分动作是在Java虚拟机之外进行的，而解释器在虚拟机的内部，所以Java程序的编译就是半独立的实现

2.基于栈的指令集与基于寄存器的指令集

基于栈的指令集主要的优点就是可移植，寄存器由硬件直接提供。
缺点：慢
