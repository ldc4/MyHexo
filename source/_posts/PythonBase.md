title: Python基础
date: 2016-06-17 14:20:32
categories:
- Python
tags:
- base
---

脚本语言还是得学一个的。
既然python这个月在TIOBE排行第4，
就选pyton来学好了

update:
学后发现Python很奇妙，
变相感觉Python其实挺难的

<!--more-->

## Python入门
python常用领域：
网络应用（网站、后台服务）
日常工具（系统管理员需要的任务脚本）
把其他语言开发的应用程序再包装起来，方便使用

缺点：
慢
不能加密

Python是跨平台的
安装Python：很傻瓜，install类型的

整个Python语言从规范到解释器都是开源的
CPython：官方解释器，由C编写的，所以叫CPython。在命令行下运行python就是启动CPython解释器
IPython：相比于CPython，增强交互方式而已，大同小异
PyPy：优势在于采用JIT技术，所以执行速度会快些
Jython：可以把Python代码编译成java的字节码
IronPython：可以把Python代码编译成.Net的字节码

Python的解释器很多，但使用最广泛的还是CPython。如果要和Java或.Net平台交互，最好的办法不是用Jython或IronPython，而是通过网络调用来交互，确保各程序之间的独立性。

### hello world

`print("hello world")`

print前不能有任何空格
linux下，加上`#!/usr/bin/env python3`，就可以直接像shell那样运行了

*.辅助工具learning.py
遇到第一个python错误：
`UnicodeDecodeError: 'utf-8' codec can't decode byte 0xbf in position 0: invalid  start byte`
问题有点诡异

### 输入和输出

print可以有多个参数，用逗号隔开，逗号会打印成一个空格来替代

例：`print("I'am","ldc4,","as weedust")`

`input()`可以让用户输入一个字符串并放到变量里
`input("这里可以是提示：")`

## Python基础

Python使用缩进来组织代码块，请务必遵守约定俗成的习惯，坚持使用4个空格的缩进。
(个人觉得这是作者的感情色彩,我感觉用TAB舒服些)

以#开头的语句是注释，注释是给人看的，可以是任意内容，解释器会忽略掉注释。
其他每一行都是一个语句，当语句以冒号:结尾时，缩进的语句视为代码块。

Python大小写敏感

### 基本数据类型
整数、浮点数、字符串、布尔值、空值

如果字符串里面有很多字符都需要转义，就需要加很多\，为了简化，Python还允许用r' '表示' '内部的字符串默认不转义
如果字符串内部有很多换行，用\n写在一行里不好阅读，为了简化，Python允许用'''...'''的格式表示多行内容

例如：
```
print(r'''I'am
long,\n
really!''')
# 输出自行脑补
```

True|False
and or not

None表示空值

### 变量
变量名必须是大小写英文、数字和_的组合，且不能用数字开头
等号=是赋值语句，可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是不同类型的变量
在python中有两种除法，一种是/，另一种是//，前一种是精确除法
Python的整数没有大小限制
Python的浮点数也没有大小限制，但是超出一定范围就直接表示为inf（无限大）。

评论中看到`print(a,b,c,seq='\n')`的用法，逗号，就使用\n来替换了
不能使用类似于这种的`print(r'\\\')`

### 字符串和编码问题
ASCII - Unicode - UTF-8

`ord()`：获取字符的数字表示
`chr()`：把编码转换成对应字符

如果知道字符的整数编码，还可以用十六进制来写
`\u4e2d\u6587`


由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。
如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。
Python对bytes类型的数据用带b前缀的单引号或双引号表示
要注意区分`'ABC'`和`b'ABC'`，前者是str，后者虽然内容显示得和前者一样，但bytes的每个字符都只占用一个字节
在bytes中，无法显示为ASCII字符的字节，用`\x##`显示
反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是bytes。要把bytes变为str，就需要用`decode()`方法


当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行
```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```
第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；
第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。
申明了UTF-8编码并不意味着你的.py文件就是UTF-8编码的，必须并且要确保文本编辑器正在使用UTF-8 without BOM编码


格式化：
`%d %f %s %x（十六进制）`

例如：
```
'%2d-%03d' % (2,1)
->' 2-001'
```
如果你不太确定应该用什么，%s永远起作用，它会把任何数据类型转换为字符串

字符串里面的%是一个普通字符, 需要转义，用`%%`来表示一个%


### list

`mylist = ['haha','heihei','hehe']`

负数索引为倒数第几个，-1表示倒数第一个。

list是一个可变的有序表

`mylist.append('yoyo')`
`mylist.insert(1,'zz')`  插入到索引为1的位置，原位置的数据往后移
`mylist.pop()`或`mylist.pop(i)`

要把某个元素替换成别的元素，可以直接赋值给对应的索引位置
list里面的元素的数据类型也可以不同，且可以嵌套list
嵌套list的元素可以通过多维数组的形式来访问

### tuple

另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改

tuple不可变

`mytuple = ('ldc4',21,weedust)`

只有1个元素的tuple定义时必须加一个逗号,，来消除歧义

tuple可以包含list,来达到可变的效果

list和tuple是Python内置的有序集合，一个可变，一个不可变。

### 条件判断
```
if xxx:
     yyy
elif xxx:
     yyy
else:
     yyy
```
注意不要少了冒号`：`
只要x是非零数值、非空字符串、非空list等，就判断为`True`，否则为`False`

### 循环
```
for x in xs:
     yyy
```
还是得注意冒号，冒号特别容易忘
`range(5)`提供一个从0到4的范围，`list(range(5))`就是[0,1,2,3,4]
in后面可以直接跟range返回值类型xrange
(python2.x是返回的list)
```
while xxx:
     yyy
```
### dict

`mydict = {'a':'ldc4','b':21,'c':'weedust',4:'yo'}`
如果key不存在，dict就会报错
要避免key不存在的错误，有两种办法，一是通过`in`判断key是否存在
二是通过dict提供的get方法，如果key不存在，可以返回None，或者自己指定的value

`mydict.pop(key)`

dict内部存放的顺序和key放入的顺序是没有关系的。

需要牢记的第一条就是**dict的key必须是不可变对象**。

需要添加新元素直接赋值就行了：

例如：`mydict[5] = 'qikenao'`

### set

要创建一个set，需要提供一个list作为输入集合

`myset = set(['a','b','c'])`

通过`add(key)`可以添加元素
通过`remove(key)`可以删除元素

`s1 & s2`
`s1 | s2`
可以做交并操作

同理`set()`参数也必须是不可变对象

字符串和整数就是不可变对象，可以放心用


## Python函数

Python有许多[内建函数](https://docs.python.org/3/library/functions.html#abs)


### 调用函数
`abs()` `max()` `int()` `float()` `str()` `bool()` `hex()`

函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个别名

### 定义函数
在Python中，定义一个函数要使用`def`语句，依次写出函数名、括号、括号中的参数和冒号`:`，然后，在缩进块中编写函数体，函数的返回值用`return`语句返回。

如果没有`return`语句，函数执行完毕后也会返回结果，只是结果为`None`。
`return None`可以简写为`return`

空函数：
如果想定义一个什么事也不做的空函数，可以用`pass`语句

参数检查：
数据类型检查可以用内置函数`isinstance()`实现
例如：
```
if not isinstance(x, (int, float)):
     raise TypeError('bad operand type')
```
python函数可以返回多个值，而这多个值是一个假象，其实是一个tuple

### 函数参数

**位置参数**，就是普通参数

**默认参数**，即给参数赋值默认值
必选参数在前，默认参数在后，否则Python的解释器会报错
有多个默认参数时，调用的时候，既可以按顺序提供默认参数
当不按顺序提供部分默认参数时，需要把参数名写上

默认参数有坑，例如：
```
def add_end(L=[]):
    L.append('END')
    return L
```
多次调用函数`add_end()`会出现多个`END`

所以，定义默认参数要牢记一点：**默认参数必须指向不变对象！**

上面修改：
```
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
```

**可变参数**

`def foo(*x):`

星号表示可变参数，在函数内部,参数`x`接收到的是一个tuple

`*x`表示把`x`这个list的所有元素作为可变参数传进去。这种写法相当有用，而且很常见


**关键字参数**

`def foo(**y):`

两个信号表示可变参数，在函数内部，参数`x`接收的是一个dict

`**x`表示把`x`这个dict的所有key-value用关键字参数传入到函数的`**y`参数,`y`是`x`的一份拷贝。(`id()`可以查看内存地址)

**命名关键字参数**

`def foo(a,b, *, c, d):`
    
命名关键字参数需要一个特殊分隔符`*`，`*`后面的参数被视为命名关键字参数
调用如下：
`foo('a','b',c='c',d='d')`

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了
例如：
`def foo(a,b,*c,d,e)`

命名关键字参数必须传入参数名，这和位置参数不同

**参数组合**

在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。
但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。

对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的


### 递归函数

递归为何这么快就能得出结果！

尾递归优化，虽然python也没有此优化

## Python高级特性

### 切片

`a[start:end:step] # start through not past end, by step`

`L=[...]` L为一个list

Python提供了切片（Slice）操作符

`L[m:n]`表示，从索引m开始取，直到索引n为止，但不包括索引n。

如果第一个索引是0，还可以省略:`L[:n]`

类似的，既然Python支持`L[-1]`取倒数第一个元素，那么它同样支持倒数切片

`L[m:n:k]` k表示每k个取一个，从m开始，取`m+xk(x>=0)`直到n

甚至什么都不写，只写`[:]`就可以原样复制一个list

tuple也是一种list,tuple也可以切片，但是操作结果还是tuple

### 迭代

当我们使用for循环时，只要作用于一个可迭代对象，for循环就可以正常运行，而我们不太关心该对象究竟是list还是其他数据类型

如何判断一个对象是可迭代对象呢？方法是通过collections模块的Iterable类型判断
```
from collections import Iterable
isinstance('abc',Iterable)
```

如果要对list实现类似Java那样的下标循环怎么办？Python内置的enumerate函数可以把一个list变成索引-元素对，这样就可以在for循环中同时迭代索引和元素本身
```
for i,value in enumerate(mylist):
```

默认是从0开始索引的，但是很多时候都是从1开始索引，可以在后面加一个参数1
`for i, value in enumerate(['A', 'B', 'C'],1)`

这里出现了多个循环变量，原理是：
`for x,y,z in [(1,2,3),(4,5,6),(7,8,9)]:`


### 列表生成式

`list(range(1,11))`

`[ x * x for x in range(1,11) ]`

for循环后面还可以加上if判断，这样我们就可以筛选出仅偶数的平方

`[x * x for x in range(1, 11) if x % 2 == 0]`

还可以使用两层循环，可以生成全排列

`[m + n for m in 'ABC' for n in 'XYZ']`

列表生成式也可以使用两个变量来生成list

### 生成器

背景：一个很大的列表会浪费很多的内存空间，所以想要使用一种能够推算出后续元素的方法

把列表生成式的`[ ]`换成`( )`,就可以得到一个generator

generator是可迭代的

创建了一个generator后，基本上永远不会调用`next()`，而是通过for循环来迭代它，并且不需要关心`StopIteration`的错误。

第二种定义生成器：带有`yield`语句的function
最难理解的就是generator和函数的执行流程不一样。函数是顺序执行，遇到`return`语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用`next()`的时候执行，遇到`yield`语句返回，再次执行时从上次返回的`yield`语句处继续执行

用for循环调用generator时，发现拿不到generator的return语句的返回值。
如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中
```
try:
     xxx
except StopIterator as e:
     xxx
     break
```
**注意break**


### 迭代器

生成器都是Iterator对象， 但list、dict、str虽然是Iterable，却不是Iterator。

把list、dict、str等Iterable变成Iterator可以使用`iter()`函数

Python的Iterator对象表示的是一个数据流，Iterator对象可以被`next()`函数调用并不断返回下一个数据，直到没有数据时抛出StopIteration错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过`next()`函数实现按需计算下一个数据，所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算。


## Python函数式编程

函数式编程就是一种抽象程度很高的编程范式，纯粹的函数式编程语言编写的函数没有变量，
因此，任意一个函数，只要输入是确定的，输出就是确定的，这种纯函数我们称之为没有副作用。
而允许使用变量的程序设计语言，由于函数内部的变量状态不确定，同样的输入，可能得到不同的输出，因此，这种函数是有副作用的。

函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数

Python对函数式编程提供部分支持。由于Python允许使用变量，因此，Python不是纯函数式编程语言。


### 高阶函数

Higher-order function

函数本身也可以赋值给变量，即：变量可以指向函数。
例：`f = abs`

函数名其实就是指向函数的变量
例如：
```
abs = 10
abs(-10)就会报错
TypeError: 'int' object is not callable
```
要恢复abs函数，请重启Python交互环境
注：由于abs函数实际上是定义在import builtins模块中的，所以要让修改abs变量的指向在其它模块也生效，要用`import builtins; builtins.abs = 10`。

既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。
例如：
```
def add(x, y, f):
    return f(x) + f(y)
```

一些高阶函数:
`map()`函数接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。

`reduce()`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算，其效果就是
```
from functools import reduce
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

`filter()`和`map()`类似，也接收一个函数和一个序列。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。

`sorted(L,key=abs,reverse=True)`


一些注意事项：
nonlocal可以修饰变量不为局部变量
python小数精度问题：http://my.oschina.net/lionets/blog/186575
如0.1+0.02 = 0.12000...001
reduce可以有第三个参数，作用是拿来补位的，就是往最顶端插一个东西


**发现一个神器**：http://www.pythontutor.com/


### 返回函数

简而言之，就是某函数f1的返回值是一个函数f2

f1的参数和变量都保存在f2中，这种被称为“闭包”的程序结构

个人理解：“外部想要访问某函数内部的东西，是不可以的。于是在某函数内部定义一个函数，该函数能访问外部的东西，然后将该函数作为返回值返回回去，于是外部可以通过返回值来访问某函数内部的东西”

f2就被称为闭包，闭包函数不是立即执行的，需要你去调用。

返回闭包时牢记的一点就是：**返回函数不要引用任何循环变量，或者后续会发生变化的变量。**

如果一定要引用循环变量怎么办？
方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变

小结：
一个函数可以返回一个计算结果，也可以返回一个函数。
返回一个函数时，牢记该函数并未执行，返回函数中不要引用任何可能会变化的变量。

### 匿名函数

`lambda x : x * x`

关键字lambda表示匿名函数，冒号前面的x表示函数参数

匿名函数有个限制，就是只能有一个表达式，不用写return，返回值就是该表达式的结果。

用匿名函数有个好处，因为函数没有名字，不必担心函数名冲突。此外，匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数

同样，也可以把匿名函数作为返回值返回

Python对匿名函数的支持有限，只有一些简单的情况下可以使用匿名函数。

### 装饰器
```
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator

```
使用装饰器：将`@log`与`@log('xxx')`修饰在要装饰的函数定义上面
例如：
```
@log
def myfunc():
     print("test")
```
其本质为：`myfunc = log(myfunc)`
姑且认为这是语法糖吧。或者类似java注解

Python除了能支持OOP的decorator外，直接从语法层次支持decorator
Python的decorator可以用函数实现，也可以用类实现。

### 偏函数

functools.partial

原理：
```
def int2(x, base=2):
    return int(x, base)
```
变成了：
```
import functools
int2 = functools.partial(int, base=2)
```
functools.partial的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。

创建偏函数时，实际上可以接收函数对象、`*args`和`**kw`这3个参数
例如：
`max2 = functools.partial(max, 10)`
实际上会把10作为`*args`的一部分自动加到左边

## python模块

在Python中，一个.py文件就称之为一个模块（Module）

例如： 一个abc.py的文件就是一个名字叫abc的模块，一个xyz.py的文件就是一个名字叫xyz的模块。

可重用性

使用模块还可以避免函数名和变量名冲突

为了避免模块名冲突，Python又引入了按目录来组织模块的方法，称为包（Package）

例如：
创建一个目录mycompany， abc.py模块的名字就变成了mycompany.abc，类似的，xyz.py的模块名变成了mycompany.xyz。

每一个包目录下面都会有一个`__init__.py`的文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录，而不是一个包。

`__init__.py`可以是空文件，也可以有Python代码，因为`__init__.py`本身就是一个模块，而它的模块名就是mycompany。

类似的，可以有多级目录，组成多级层次的包结构。

自己创建模块时要注意命名，不能和Python自带的模块名称冲突。

### 使用模块
```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

' a test module '

__author__ = 'xxx'

import sys

def test():
    args = sys.argv
    if len(args)==1:
            print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':
    test()
```
第1行和第2行是标准注释
第4行是一个字符串，表示模块的文档注释，任何模块代码的第一个字符串都被视为模块的文档注释
第6行使用__author__变量把作者写进去，这样当你公开源代码后别人就可以瞻仰你的大名
以上就是Python模块的标准文件模板

导入sys模块后，我们就有了变量sys指向该模块，利用sys这个变量，就可以访问sys模块的所有功能
```
if __name__=='__main__':
    test()
```
当我们在命令行运行hello模块文件时，Python解释器把一个特殊变量`__name__`置为`__main__`，而如果在其他地方导入该hello模块时，if判断将失败，因此，这种if测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。

`import xxx`

作用域

在一个模块中，我们可能会定义很多函数和变量，但有的函数和变量我们希望给别人使用，有的函数和变量我们希望仅仅在模块内部使用。在Python中，是通过_前缀来实现的。

正常的函数和变量名是公开的（public），可以被直接引用，比如：abc，x123，PI等

类似`__xxx__`这样的变量是特殊变量，可以被直接引用，但是有特殊用途，比如上面的`__author__`，`__name__`就是特殊变量，hello模块定义的文档注释也可以用特殊变量`__doc__`访问，我们自己的变量一般不要用这种变量名

类似`_xxx`和`__xxx`这样的函数或变量就是非公开的（private），**不应该**被直接引用，比如`_abc`，`__abc`等

### 安装第三方模块

PyPI - the Python Package Index
https://pypi.python.org/pypi

windows下是pip命令
Linux下是pip3命令

模块搜索路径
搜索路径存放在sys模块的path变量中

一是直接修改sys.path，添加要搜索的目录：
```
import sys
sys.path.append('/Users/michael/my_py_scripts')
```
这种方法是在运行时修改，运行结束后失效。

第二种方法是设置环境变量PYTHONPATH，该环境变量的内容会被自动添加到模块搜索路径中。
```
# 导入mode，import与from…import的不同之处在于，简单说：
# 如果你想要直接输入argv变量到你的程序中而每次使用它时又不想打sys，
# 则可使用：from sys import argv
# 一般说来，应该避免使用from..import而使用import语句，
# 因为这样可以使你的程序更加易读，也可以避免名称的冲突
```
小结：
我们可以通过import来导入多个模块，用“,”（逗号）分隔。
注意`import`与`from..import..`

## Python面向对象编程

面向对象编程的设计思想是源于自然界的

面向过程的程序设计把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。

而面向对象的程序设计把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。

在Python中，所有数据类型都可以视为对象，当然也可以自定义对象。
自定义的对象数据类型就是面向对象中的类（Class）的概念。

面向对象的抽象程度又比函数要高，因为一个Class既包含数据，又包含操作数据的方法。

### 类和实例

```
class Studeng(object):
     pass
```

定义类是通过class关键字

(object) 表示该类是从那个类继承下来的，可要可不要，默认就是它

由于类可以起到模板的作用，通过定义一个特殊的`__init__`方法，可以理解为java的构造方法

注意到`__init__`方法的第一个参数永远是self，表示创建的实例本身，因此，在`__init__`方法内部，就可以把各种属性绑定到self，因为self就指向创建的实例本身。注意这里的self只是参数的名称而已，我们也可以使用java用惯了的this

有了`__init__`方法，在创建实例的时候，就不能传入空的参数了，必须传入与`__init__`方法匹配的参数，但self不需要传，Python解释器自己会把实例变量传进去。由此可以看出不支持重载。

和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数永远是实例变量self，并且，调用时，不用传递该参数。
除此之外，类的方法和普通函数没有什么区别，所以，你仍然可以用默认参数、可变参数、关键字参数和命名关键字参数。

数据封装
就是属性和方法，对应于数据和数据处理

2.访问限制

通过双下划线`__`来修饰为私有变量

而前后都有双下划线的是特殊变量

只有一个下划线`_`修饰的表示：**你可以访问到，但是不推荐外部访问**

双下划线开头的实例变量是不是一定不能从外部访问呢？
其实也不是。不能直接访问`__name`是因为Python解释器对外把`__name`变量改成了`_Student__name`，所以，仍然可以通过`_Student__name`来访问`__name`变量

Python本身没有任何机制阻止你干坏事，一切全靠自觉。这个6

3.继承和多态

类名后面的括号里的内容就是父类/超类/基类

封装、继承、多态。

除了这些以外：
对于Python这样的动态语言来说，则不一定需要传入Animal类型。我们只需要保证传入的对象有一个run()方法就可以了
```
class Timer(object):
    def run(self):
        print('Start...')
```
这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。


4.获取对象信息

判断对象类型，使用`type()`函数
`type()`函数返回type这种类型，`<class 'ccc'>`

```
import types
types.FunctionType
types.BuildinFunctionType
```

要判断class的类型，使用`isinstance()`函数
例如：
```
d = Dog()
print(isinstance(d,Animal))
```
还可以判定一个变量是否是某些类型中的一种
`isinstance([1,2,3],(list,tuple))`


要获得一个对象的所有属性和方法，可以使用`dir()`函数

`__xx__`都是有特殊用途的变量：
在Python中，如果你调用`len()`函数试图获取一个对象的长度，实际上，在`len()`函数内部，它自动去调用该对象的`__len__()`方法

另外还有
`setattr()`
`getattr()`
`hasattr()`

5.实例属性和类属性

实例属性可以通过实例变量或者self来绑定

类属性，可以直接定义在类中

## python面向对象高级编程

## 使用`__slots__`（属性添加限制）

给实例添加方法：
```
class Student:pass
def foo(self):pass
from types import MethodType
s.foo = MethodType(foo,s)
```
给类添加方法：
`Student.foo = foo`

`__slots__`特殊变量可以用来限制class实例能够添加的属性

例如：
```
class Student(object):
     __slots__ = ('name','age')
```
使用`__slots__`要注意，`__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的
除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`


## 使用`@property`(getter与setter方法)

把一个getter方法变成属性，只需要加上`@property`就可以了
`@property`本身又创建了另一个装饰器`@score.setter`， 负责把一个setter方法变成属性赋值

例如：
```
class Student(object):
    @property
    def birth(self):
        return self.__birth
    @birth.setter
    def birth(self,value):
        self.__birth = value
```
## 多重继承

就是在类后面的括号中用逗号分隔多个父类。。。

例如：`class Dog(Mammal, Runnable)`

Python中所谓的**MixIn**就相当于java中实现了方法的interface吧，但毕竟是多继承，通过名称后缀来修饰，并没有什么实质的意义。

## 定制类

`__str__` 相当于java的toString方法

当在python交互环境中，不用print来打印的话，还是显示原来的模样。
因为默认会去调用`__repr__`

`__iter__` 实现该方法，就能被forin迭代

这里直接返回self对象，因为还要实现一个`__next__`方法，该方法抛出`StopIteration()`来终止迭代

例如：
```
class Fib(object):
    def __init__(self):
        self.a,self.b = 0,1
    def __iter__(self):
        return self
    def __next__(self):
        self.a,self.b = self.b,self.a+self.b
        if self.a > 100000:
            raise StopIteration();
        return self.a

for i in Fib():
    print(i)
```

`__getitem__` 可以像list那样，利用下标来取出元素

例如：
```
class Fib(object):
    def __getitem__(self, n):
        a,b = 1,1
        for x in range(n):
            a,b = b,a+b
        return a

f = Fib()
print(f[0],f[1],f[2],f[3])
```
参数n表示下标

`__getitem__()`传入的参数可能是一个int，也可能是一个切片对象slice

例如：
```
if isinstance(n,slice):
    start = n.start
    stop = n.stop
    if start is None:
        start = 0
    a,b = 1,1
    L = []
    for x in range(stop):
        if x >= start:
            L.append(a)
        a,b = b,a+b
    return L
```
这个只对[:n]做过处理，其他的情况没有处理。

与之对应的是`__setitem__()`方法，把对象视作list或dict来对集合赋值。
最后，还有一个`__delitem__()`方法，用于删除某个元素。

这完全归功于动态语言的“鸭子类型”

当调用不存在的属性时，比如score，Python解释器会试图调用`__getattr__(self, 'score')`来尝试获得属性，这样，我们就有机会返回score的值。也可以返回函数。

例如：
```
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
```

**注意，只有在没有找到属性的情况下，才调用`__getattr__`**，已有的属性，比如name，不会在`__getattr__`中查找。

此外，注意到任意调用如`s.abc`都会返回`None`，这是因为我们定义的`__getattr__`默认返回就是`None`。要让class只响应特定的几个属性，我们就要按照约定，抛出`AttributeError`的错误

`__call__` 让自己变成可调用的

通过`callable()`方法可以判断对象是否可调用

对实例进行直接调用就好比对一个函数进行调用一样，所以你完全可以把对象看成函数，把函数看成对象，因为这两者之间本来就没啥根本的区别。如果你把对象看成函数，那么函数本身其实也可以在运行期动态创建出来，因为类的实例都是运行期创建出来的，这么一来，我们就模糊了对象和函数的界限。

其他定制方法：http://docs.python.org/3/reference/datamodel.html#special-method-names

## 使用枚举类

```
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
```
python提供了Enum类来创建枚举类

另外，枚举所有成员：
```
for name, member in Month.__members__.items():
    print(name, '=>', member, ',', member.value)
```
每个枚举成员都有个对应的值，默认从1开始递增

除了使用工具类来定义，还可以继承Enum定义：
```
from enum import Enum, unique

@unique
class Weekday(Enum):
    Sun = 0 # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6
```
@unique装饰器，使用来检验value的唯一性


获取枚举值：
```
Weekday.Sun
Weekday['Sun']
Weekday(0)
```
## 使用元类

`type()`函数既可以检查一个变量或类型的类型，还可以定义一个类型（或者说称为类型的类对象）

例如：
```
def fn(self, name='world'): # 先定义函数
     print('Hello, %s.' % name)
Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
```

要创建一个class对象，`type()`函数依次传入3个参数：
-class的名称；
-继承的父类集合，注意Python支持多重继承，如果只有一个父类，别忘了tuple的单元素写法；
-class的方法名称与函数绑定，这里我们把函数fn绑定到方法名hello上。

通过`type()`函数创建的类和直接写class是完全一样的，因为Python解释器遇到class定义时，仅仅是扫描一下class定义的语法，然后调用`type()`函数创建出class。

**metaclass**

除了使用`type()`动态创建类以外，要控制类的创建行为，还可以使用`metaclass`。

先定义metaclass，就可以创建类，最后创建实例
```
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self,value:self.append(value)
        return type.__new__(cls, name, bases, attrs)

class MyList(list,metaclass=ListMetaclass):
    pass
```
当我们传入关键字参数metaclass时，魔术就生效了，它指示Python解释器在创建MyList时，要通过`ListMetaclass.__new__()`来创建，在此，我们可以修改类的定义，比如，加上新的方法，然后，返回修改后的定义。

`__new__()`方法接收到的参数依次是：
- 当前准备创建的类的对象；
- 类的名字；
- 类继承的父类集合；
- 类的方法集合。

ORM框架

## Python错误、调试和测试

### 错误处理

`try...except...finally...`

可以有多个except

`except XXXError as e`

e为异常对象

另外，可以在except后面用else语句来表示没有错误

例如：
```
try:
    num = 1/int('1')
    print(num)
except ZeroDivisionError as e:
    print('exception =',e)
except ValueError as e:
    print('exception =',e)
else:
    print("no problem")
finally:
    print("finally...")
print("后续")
```

所有错误类型都继承自`BaseException`
内建异常层次关系：
https://docs.python.org/3/library/exceptions.html#exception-hierarchy
规律：`BaseException -> Exception -> XXXError/XXXWarning`


调用堆栈
`Traceback <most recent call last>`

记录错误
```
import logging
loggint.exception(e)
```

抛出错误

先定义一个错误的class，选择好继承关系，然后，用raise语句抛出一个错误的实例
```
class MyError(Exception):pass
raise MyError('xxx')
```
只有在必要的时候才定义我们自己的错误类型。
如果可以选择Python已有的内置的错误类型（比如ValueError，TypeError），尽量使用Python内置的错误类型。

raise语句如果不带参数，就会把当前错误原样抛出。

在捕获异常后可以继续抛出一个异常来转换异常
```
def handle_file(f):
    try:
        ...
    except IOError as e:
        ...
    except BaseException:
        raise
```

### 调试

断言 `assert xxx , 'yyy'`

如果`xxx`为`False`，就会抛出一个`AssertionError('yyy')`

在运行python的时候，加上`-O`参数可以关闭`assert`，关闭后assert语句就等同于pass

logging

logging有debug,info,warning,error等几个级别
`logging.basicConfig(level=logging.INFO)`

pdb(python debugger)

在执行程序的时候添加参数 `-m pdb`，进入pdb命令行接口

`n`表示下一步
`l`表示列出代码
`p 变量名`可以查看变量值


在代码中，
```
import pdb
pdb.set_trace() #设置断点
```
在程序运行时，自动进入pdb并到设置断点的代码

`c`表示继续执行

目前比较好的Python IDE有PyCharm：
http://www.jetbrains.com/pycharm/
另外，Eclipse加上pydev插件也可以调试Python程序。
http://www.pydev.org/

“虽然用IDE调试起来比较方便，但是最后你会发现，logging才是终极武器。”

```
命令 解释
break 或 b 设置断点
continue 或 c 继续执行程序
list 或 l 查看当前行的代码段
step 或 s 进入函数
return 或 r 执行代码直到从当前函数返回
exit 或 q 中止并退出
next 或 n 执行下一行
p 变量名 打印变量的值
help 帮助
```

### 单元测试

继承`unittest.TestCase`类
以test开头的被认为是测试方法
和java类似的内置断言方法

一种重要的断言就是期待抛出指定类型的Error，比如通过d['empty']访问不存在的key时，断言会抛出KeyError：
```
with self.assertRaises(KeyError):
    value = d['empty']
```

运行测试：`unittest.main()`

setUp与tearDown


### 文档测试

Python内置的“文档测试”（doctest）模块可以直接提取注释中的代码并执行测试。
doctest严格按照Python交互式命令行的输入和输出来判断测试结果是否正确。只有测试异常的时候，可以用`...`表示中间一大段烦人的输出。

例如：
```
def fact(n):
	'''
	this is exam
	
	>>> fact(0)
	Traceback (most recent call last):
	  ...
	ValueError
	>>> fact(1)
	1
	>>> fact(2)
	2
	>>> fact(3)
	6
	>>> fact(4)
	24
	>>> fact(5)
	120
	>>>
	'''
	if n<1:
		raise ValueError()
	if n == 1:
		return 1
	return n*fact(n-1)

if __name__=='__main__':
    import doctest
    doctest.testmod()
```
另外要注意的是，单元测试unittest.main()后面的代码不会执行
-v 参数 可以查看测试过程


## IO编程

“如果是服务员跑过来找到你，这是回调模式，如果服务员发短信通知你，你就得不停地检查手机，这是轮询模式。”

在磁盘上读写文件的功能都是由操作系统提供的，现代操作系统不允许普通的程序直接操作磁盘，所以，读写文件就是请求操作系统打开一个文件对象（通常称为文件描述符），然后，通过操作系统提供的接口从这个文件对象中读取数据（读文件），或者把数据写入这个文件对象（写文件）。

### 文件读写
```
try:
    f = open('./2016年6月29日.txt','r')
    print(f.read())
finally:
    if f:
        f.close()
```

### with代替finally
```
with open('2016年6月28日.txt','r') as f:
    print(f.read())
```
另外`f.readline()`、`f.read(size)`、`f.readlines()`

`file-like object`:只要有read方法就行了
StringIO就是在内存中创建的`file-like Object`，常用作临时缓冲。

打开二进制文件，用`'rb'`模式

`open`方法有`encoding`关键字参数
例如：
```
with open('test.txt','r',encoding='gb2312') as f:
    print(f.read())
```

`errors='ignore'`


write方法：open的时候用'w','wb'模式
```
# 'r'    open for reading (default)
# 'w'    open for writing, truncating the file first
# 'x'    open for exclusive creation, failing if the file already exists
# 'a'    open for writing, appending to the end of the file if it exists
# 'b'    binary mode
# 't'    text mode (default)
# '+'    open a disk file for updating (reading and writing)
# 'U'    universal newlines mode (deprecated)
```

### StringIO和BytesIO

要把str写入StringIO，我们需要先创建一个StringIO，然后，像文件一样写入即可

```
from io import StringIO
f = StringIO()
f.write('hello')
f.getvalue()
```

write方法返回写入的字符数

也可以通过readline方式读取
例如：
```
f = StringIO('hello!\nHi!\nGoodbye!')
while True:
    s = f.readline()
    if s == '':
        break
    print(s.strip())
```
**但是要注意不要先写后读，那样没有效果**

StringIO操作的只能是str，如果要操作二进制数据，就需要使用BytesIO。

BytesIO用法与StringIO一样：
```
from io import BytesIO
f = BytesIO()
f.write('临独沉疯'.encode('utf-8'))
print(f.getvalue())

f = BytesIO(b'\xe4\xb8\xb4\xe7\x8b\xac\xe6\xb2\x89\xe7\x96\xaf')
print(f.read().decode('utf-8'))
```

### 操作文件和目录

Python内置的os模块也可以直接调用操作系统提供的接口函数。

`os.name`如果返回`nt`表示windows,如果返回`posix`表示Unix、Linux、Mac OS X

注意`uname()`函数在Windows上不提供，也就是说，os模块的某些函数是跟操作系统相关的。

环境变量：`os.environ`

要获取某个环境变量的值：`os.environ.get('key'[,'default'])`


操作文件和目录的函数一部分放在os模块中，一部分放在os.path模块中，这一点要注意一下。

```
os.path.abspath('.')
os.path.join('/aaa','bbb')
os.path.split('/aaa/bbb')
os.mkdir('xxx')
os.rmdir('yyy')
os.path.splitext('aaa.txt')
os.rename()
os.remove()
```

幸运的是`shutil`模块提供了`copyfile()`的函数，你还可以在`shutil`模块中找到很多实用函数，它们可以看做是os模块的补充。

可以通过列表生成式一行代码列出当前目录下的文件夹，或者py文件
```
print([x for x in os.listdir('.') if os.path.isdir(x)])
print([x for x in os.listdir('.') if os.path.splitext(x)[1]=='.py'])
```
使用`isdir(x)`需要注意,`x`需要用绝对路径，不然默认用工作路径与相对路径计算出路径，会出现找不到这个目录，一样`False`。

### 序列化

在python被称为pickling

Python提供了`pickle`模块来实现序列化。

`pickle.dumps()`方法把任意对象序列化成一个bytes

`pickle.dump()`直接把对象序列化后写入一个`file-like Object`

用`pickle.loads()`方法反序列化出对象，也可以直接用`pickle.load()`方法从一个file-like Object中直接反序列化出对象。

例如：
```
import pickle

d = dict(name='Bob', age=20, score=88)

print(pickle.dumps(d))
print(pickle.loads(pickle.dumps(d)))


f = open('dump.txt', 'wb')
pickle.dump(d, f)
f.close()

f = open('dump.txt','rb')
x = pickle.load(f)
f.close()
print(x)
```

Pickle的问题和所有其他编程语言特有的序列化问题一样，就是它只能用于Python，并且可能不同版本的Python彼此都不兼容，因此，只能用Pickle保存那些不重要的数据，不能成功地反序列化也没关系。

*序列化成JSON*

Python内置的`json`模块提供了非常完善的Python对象到JSON格式的转换。

```
import json
j = json.dumps(d)
```

j就是一个json字符串

其他和pickle都一样。

*json进阶*

把class序列化为json对象：

官方API：https://docs.python.org/3/library/json.html#json.dumps

可选参数default就是把任意一个对象变成一个可序列为JSON的对象，我们只需要为Student专门写一个转换函数，再把函数传进去即可
```
def student2dict(std):
    return {
        'name': std.name,
        'age': std.age,
        'score': std.score
    }
```
同样loads也提供了object_hook参数
```
json_str = json.dumps(s,default=lambda obj:obj.__dict__)
print(json.loads(json_str, object_hook=dict2student))
```

## 进程和线程

### 多进程

`os.fork()`
Unix/Linux操作系统提供了一个fork()系统调用，它非常特殊。普通的函数调用，调用一次，返回一次，但是fork()调用一次，返回两次，因为操作系统自动把当前进程（称为父进程）复制了一份（称为子进程），然后，分别在父进程和子进程内返回
子进程永远返回0，而父进程返回子进程的ID。这样做的理由是，一个父进程可以fork出很多子进程，所以，父进程要记下每个子进程的ID，而子进程只需要调用`getppid()`就可以拿到父进程的ID。
**感觉os模块，对windows的支持不太好**

多进程程序模块：`multiprocessing`

`multiprocessing`模块提供了一个`Process`类来代表一个进程对象

```
from multiprocessing import Process
def foo(some):
p = Process(target=foo, args=('some',))
p.start()
#p.join()
```

*Pool*

如果要启动大量的子进程，可以用进程池（Pool）的方式批量创建子进程

```
from multiprocessing import Pool
p = Pool(8)
for i in range(8):
     p.apply_async(foo, args=('some',))
p.close()
p.join()
```

Pool的默认大小是CPU的核数
对Pool对象调用`join()`方法会等待所有子进程执行完毕，调用`join()`之前必须先调用`close()`，调用`close()`之后就不能继续添加新的`Process`了

*子进程*

```
import subprocess

print('$ ping www.python.org')
r = subprocess.call(['ping','www.python.org'])
print('Exit code:', r)

print('$ nslookup')
p = subprocess.Popen(['nslookup'], stdin=subprocess.PIPE, stdout=subprocess.PIPE)
output, err = p.communicate(b'set type=A\npython.org\nexit\n')
print(output.decode('gbk'))
print('Exit code', p.returncode)
```
输入通过`communicate`方法来，输出记得解码

*进程间通信*

Process之间肯定是需要通信的，操作系统提供了很多机制来实现进程间的通信。
Python的`multiprocessing`模块包装了底层的机制，提供了`Queue`、`Pipes`等多种方式来交换数据。
由于Windows没有fork调用，因此，multiprocessing需要“模拟”出fork的效果，父进程所有Python对象都必须通过pickle序列化再传到子进程去，所有，如果multiprocessing在Windows下调用失败了，要先考虑是不是pickle失败了。


### 多线程

Python的标准库提供了两个模块：`_thread`和`threading`，`_thread`是低级模块，`threading`是高级模块，对`_thread`进行了封装。绝大多数情况下，我们只需要使用`threading`这个高级模块。

```
t = threading.Thread(target=foo,name='FooThread')
t.start()
```

*Lock*

创建一个锁就是通过`lock = threading.Lock()`
然后在需要上锁的地方`lock.acquire()`，最后在finally中释放锁`lock.release()`

例如：
```
balance = 0
lock = threading.Lock()

def run_thread(n):
    for i in range(100000):
        # 先要获取锁:
        lock.acquire()
        try:
            # 放心地改吧:
            change_it(n)
        finally:
            # 改完了一定要释放锁:
            lock.release()
```

**注意**：因为Python的线程虽然是真正的线程，但解释器执行代码时，有一个GIL锁：`Global Interpreter Lock`，任何Python线程执行前，必须先获得GIL锁，然后，每执行100条字节码，解释器就自动释放GIL锁，让别的线程有机会执行。这个GIL全局锁实际上把所有线程的执行代码都给上了锁，所以，多线程在Python中只能交替执行，即使100个线程跑在100核CPU上，也只能用到1个核。

GIL是Python解释器设计的历史遗留问题，通常我们用的解释器是官方实现的CPython，要真正利用多核，除非重写一个不带GIL的解释器。

所以，在Python中，可以使用多线程，但不要指望能有效利用多核。如果一定要通过多线程利用多核，那只能通过C扩展来实现，不过这样就失去了Python简单易用的特点。

不过，也不用过于担心，Python虽然不能利用多线程实现多核任务，但可以通过多进程实现多核任务。多个Python进程有各自独立的GIL锁，互不影响。

醉了。

### ThreadLocal

`threading.Local()`放回一个ThreadLocal对象，可以理解为key为thread的dict。

例如：
```
local_school = threading.local()
local_schooll.student = Studen('ldc4',21)
std = local_school.student
```
ThreadLocal最常用的地方就是为每个线程绑定一个数据库连接，HTTP请求，用户身份信息等，这样一个线程的所有调用到的处理函数都可以非常方便地访问这些资源。

### 进程VS线程

Master-Worker

Apache使用的多进程模型

IIS使用的多线程模型->多进程+多线程模型

线程切换代价

Python适合IO密集型

对应到Python语言，单进程的异步编程模型称为协程，有了协程的支持，就可以基于事件驱动编写高效的多任务程序。

### 分布式进程

Python的`multiprocessing`模块不但支持多进程，其中`managers`**子模块**还支持把多进程分布到多台机器上。

例如：
```
import random, time, queue
from multiprocessing.managers import BaseManager
from multiprocessing import freeze_support
task_queue = queue.Queue()
result_queue = queue.Queue()

class QueueManager(BaseManager):
    pass

def return_task_queue():
    return task_queue
def return_result_queue():
    return result_queue

def test():   
    QueueManager.register('get_task_queue', callable=return_task_queue)
    QueueManager.register('get_result_queue', callable=return_result_queue)

    manager = QueueManager(address=('127.0.0.1', 5000), authkey=b'abc')

    manager.start()


    task = manager.get_task_queue()
    result = manager.get_result_queue()

    for i in range(10):
        n = random.randint(0,1000)
        print('Put task %d...' % n)
        task.put(n)

    print('Try get results...')
    for i in range(10):
        try:
            r = result.get(timeout=10)
            print('Result: %s' % r)
        except queue.Empty:
            print('result queue is empty')

    manager.shutdown()
    print('master exit.')


if __name__=='__main__':
    freeze_support()
    test()
```
```
import time, sys, queue
from multiprocessing.managers import BaseManager

class QueueManager(BaseManager):
    pass

QueueManager.register('get_task_queue')
QueueManager.register('get_result_queue')

server_addr = '127.0.0.1'
manager = QueueManager(address=(server_addr, 5000), authkey=b'abc')
manager.connect()

task = manager.get_task_queue()
result = manager.get_result_queue()

for i in range(10):
    try:
        n = task.get(timeout=10)
        print('run task %d * %d...' % (n,n))
        r = '%d * %d = %d' %(n, n, n*n)
        time.sleep(1)
        result.put(r)
    except queue.Empty:
        print('task queue is empty.')

print('worker exit.')
```

## 正则表达式

Python提供re模块
```
import re
test = '用户输入的字符串'
if re.match(r'正则表达式', test):
    print('ok')
else:
    print('failed')
```
切分字符串
```
>>> re.split(r'[\s\,\;]+', 'a,b;; c  d')
['a', 'b', 'c', 'd']
```
分组
```
m = re.match(xxx,yyy)
m.groups()
```
正则分组就是小括号，其中分组序号从1开始。分组0表示原字符串。深度优先的顺序


贪婪匹配

`？` `+` `*` 默认是贪婪的，即尽可能匹配多的。在后面加一个`？`由贪婪模式变成非贪婪模式


编译
```
m = re.complie(r'xxx')
m.match('yyy')
```

## 内建模块

### datetime

*获取当前日期和时间:*
```
from datetime import datetime
print(datetime.now())
```
注意到`datetime`是模块，`datetime`模块还包含一个`datetime`类，通过`from datetime import datetime`导入的才是`datetime`这个类。

通过参数构造一个指定的`datetime`:
```
dt = datetime(2016, 7, 7, 8, 5, 0, 123)
```

*datetime转换为timestamp*

1970年1月1日 00:00:00 UTC+00:00时区的时刻称为epoch time，记为0（1970年以前的时间timestamp为负数），当前时间就是相对于epoch time的秒数，称为timestamp。

`dt.timestamp()`

某些编程语言（如Java和JavaScript）的timestamp使用整数表示毫秒数，这种情况下只需要把timestamp除以1000就得到Python的浮点表示方法。

*timestamp转换为datetime*

要把timestamp转换为datetime，使用datetime提供的`fromtimestamp()`方法
例如：
```
datetime.fromtimestamp(1467849900.0)
```
datetime是有时区的，上述转换是在timestamp和本地时间做转换。

转换为UTC标准时区
```
datetime.utcfromtimestamp(1467849900.0)
```

*str转换为datetime*
```
datetime.strptime('16-7-7 10:05:00', '%y-%m-%d %H:%M:%S')
```

*datetime转换为str*
```
datetime.now().strftime('%a, %b %d %H:%M')
```
https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior

*datetime加减*

```
from datetime import datetime
datetime.now() + timedelta(days=2,hours=10)
```
很神奇！！！

*本地时间转换成UTC时间*
```
from datetime import timezone

now = datetime.now()
dt = now.replace(tzinfo=timezone(timedelta(hours=12)))
print(dt.tzinfo)
```

*时区转换*
```
utc_dt = datetime.utcnow().replace(tzinfo=timezone.utc)
bj_dt = utc_dt.astimezone(timezone(timedelta(hours=8)))
```
时区转换的关键在于，拿到一个datetime时，要获知其正确的时区，然后强制设置时区，作为基准时间。
利用带时区的datetime，通过`astimezone()`方法，可以转换到任意时区。


### collections

*namedtuple*
```
from collections import namedtuple
Point = namedtuple('Point', ['x','y'])
p = Point(1, 2)
print(p.x,p.y)
```

*deque*
```
from collections import deque
q = deque(['x','y','z'])
q.append('a')
q.appendleft('b')
print(q)
```

*defaultdict*
```
from collections import defaultdict
dd = defaultdict(lambda: 'No')
dd['key'] = 'abc'
print(dd['key'])
print(dd['nokey'])
```

*OederedDict*
```
from collections import OrderedDict
d = dict([('a',1),('b',2),('c',3)])
print(d)
od = OrderedDict([('b',2),('a',1),('c',3)])
print(od)
od = OrderedDict()
od['x'] = 1
od['z'] = 2
od['y'] = 3
print(od)
```

这里踩了一个坑:
```
class MyDict(OrderedDict):

    def __init__(self, list, capacity):
        self._capacity = capacity
        super(MyDict,self).__init__(list)

    def __setitem__(self, key, value):
        containsKey = 1 if key in self else 0
        if len(self) - containsKey >= self._capacity:
            last = self.popitem(last=False)
            print('remove:', last)
        if containsKey:
            del self[key]
            print('set:', (key, value))
        else:
            print('add:', (key, value))
        OrderedDict.__setitem__(self, key, value)
```
__init__的两个语句不能交换


*Counter*

Counter是一个简单的计数器，例如，统计字符出现的个数
Counter实际上也是dict的一个子类
```
from collections import Counter
c = Counter()
for ch in 'programming':
    c[ch] = c[ch] + 1

print(c)
```

### base64

Base64是一种用64个字符来表示任意二进制数据的方法

Base64的原理很简单，首先，准备一个包含64个字符的数组
`['A', 'B', 'C', ... 'a', 'b', 'c', ... '0', '1', ... '+', '/']`
然后，对二进制数据进行处理，每3个字节一组，一共是3x8=24bit，划为4组，每组正好6个bit

这样我们得到4个数字作为索引，然后查表，获得相应的4个字符，就是编码后的字符串。

所以，Base64编码会把3字节的二进制数据编码为4字节的文本数据，长度增加33%，好处是编码后的文本数据可以在邮件正文、网页等直接显示。

如果要编码的二进制数据不是3的倍数，最后会剩下1个或2个字节怎么办？Base64用`\x00`字节在末尾补足后，再在编码的末尾加上1个或2个`=`号，表示补了多少字节，解码的时候，会自动去掉。

Base64是一种任意二进制到文本字符串的编码方法，常用于在URL、Cookie、网页中传输少量二进制数据。

例如：
```
import base64
b = base64.b64encode(b'ldc4 as weedust')
print(b)
print(base64.b64decode(b))

b = base64.urlsafe_b64encode(b'\xb7\x1d\xfb\xef\xff')
print(b)
print(base64.urlsafe_b64decode(b))
```
`urlsafe_b64encode`是把`+`，`/`换成`-`，`_`

由于`=`字符也可能出现在Base64编码中，但`=`用在URL、Cookie里面会造成歧义，所以，很多Base64编码后会把`=`去掉

去掉`=`后怎么解码呢？因为Base64是把3个字节变为4个字节，所以，Base64编码的长度永远是4的倍数，因此，需要加上`=`把Base64字符串的长度变为4的倍数，就可以正常解码了。

### struct

Python提供了一个`struct`模块来解决bytes和其他二进制数据类型的转换

`struct`的`pack`函数把任意数据类型转换为bytes
例如：
```
import struct
a = struct.pack('>I', 10240099)
print(a)
```
`>I`为指令，`>`表示Big-endian，`I`表示4字节无符号整数

`unpack`则相反
例如：
```
b = struct.unpack('>IH', b'\xf0\xf0\xf0\xf0\x80\x80')
print(b)
```
`struct`模块定义的数据类型可以参考Python官方文档：
https://docs.python.org/3/library/struct.html#format-characters


### hashlib

```
import hashlib

md5 = hashlib.md5()
md5.update('ldc4 as weedust.'.encode('utf-8'))
md5.update('可以调用很多次'.encode('utf-8'))
print(md5.hexdigest())

sha1 = hashlib.sha1()
sha1.update('ldc4 as weedust.'.encode('utf-8'))
sha1.update('可以调用很多次'.encode('utf-8'))
print(sha1.hexdigest())
```
加盐

### itertools

`itertools.count(1)` 返回从1开始的无限递增迭代器
`itertools.cycle('ABC')` 返回ABC无限循环迭代器
`itertools.repeat('A',3)` 返回重复3次A迭代器

无限序列虽然可以无限迭代下去，但是通常我们会通过`takewhile()`等函数根据条件判断来截取出一个有限的序列

例如：
```
natuals = itertools.count(1)
ns = itertools.takewhile(lambda x:x<=10, natuals)
print(list(ns))
```

`chain()`可以把一组迭代对象串联起来，形成一个更大的迭代器

例如：
```
for c in itertools.chain('ABC','XYZ'):
    print(c)
```

`groupby()`把迭代器中相邻的重复元素挑出来放在一起

例如：
```
for key,group in itertools.groupby('AAABBBCCDBBDDJJDJ'):
        print(key,list(group))

for key,group in itertools.groupby('AaaBcbBCccc',lambda x:x.upper()):
        print(key,list(group))
```

### XML

解析XML时，注意找出自己感兴趣的节点，响应事件时，把节点数据保存起来。解析完毕后，就可以处理数据。
```
from xml.parsers.expat import ParserCreate

class DefaultSaxHandler(object):
    def start_element(self, name, attrs):
        print('start:',name,str(attrs))
    def end_element(self, name):
        print('end:',name)
    def char_data(self, text):
        print('data:',text)

xml = r'''<?xml version="1.0"?>
<ol>a
b    <li><a href="/python">Python</a></li>c
d    <li><a href="/ruby">Ruby</a></li>e
</ol>
'''
#需要注意的是读取一大段字符串时，CharacterDataHandler可能被多次调用，所以需要自己保存起来，在EndElementHandler里面再合并。
handler = DefaultSaxHandler()
parser = ParserCreate()
parser.StartElementHandler = handler.start_element
parser.EndElementHandler = handler.end_element
parser.CharacterDataHandler = handler.char_data
parser.Parse(xml)
```

### HTMLParser
```
from html.parser import HTMLParser

class MyHTMLParser(HTMLParser):
    def handle_starttag(self, tag, attrs):
        print('<%s>' % tag)

    def handle_endtag(self, tag):
        print('</%s>' % tag)

    def handle_startendtag(self, tag, attrs):
        print('<%s/>' % tag)

    def handle_data(self, data):
        print(data)

    def handle_comment(self, data):
        print('<!--', data, '-->')

    def handle_entityref(self, name):
        print('&%s;' % name)

    def handle_charref(self, name):
        print('&#%s;' % name)

parser = MyHTMLParser()
parser.feed('''
<html>
    <head></head>
    <body>
    <!-- test html parser -->
        <p>Some<a href=\"#\">html</a> HTML&lt;tutorial...<br>END</p>
    </body>
</html>
''')

需要注意的是：&nbsp; 在cmd,gbk编码不了。
```

### urllib

```
#模拟iPhone 6去请求豆瓣首页

from urllib import request

req = request.Request('http://www.douban.com')
req.add_header('User-Agent','Mozilla/6.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/8.0 Mobile/10A5376e Safari/8536.25')
with request.urlopen(req) as f:
    data = f.read()
    print('Status:', f.status, f.reason)
    for k,v in f.getheaders():
        print('%s:%s' % (k, v))
    print('Data:', data.decode('utf-8'))
```

如果要以POST发送一个请求，只需要把参数data以bytes形式传入urlopen即可

你可以自己构造，也可以利用parse
```
from urllib import request,parse
login_data = parse.urlencode([
    ('username', email),
    ('password', passwd),
    ('entry', 'mweibo'),
    ('client_id', ''),
    ('savestate', '1'),
    ('ec', ''),
    ('pagerefer', 'https://passport.weibo.cn/signin/welcome?entry=mweibo&r=http%3A%2F%2Fm.weibo.cn%2F')
])
```

代理：
```
proxy_handler = urllib.request.ProxyHandler({'http': 'http://www.example.com:3128/'})
proxy_auth_handler = urllib.request.ProxyBasicAuthHandler()
proxy_auth_handler.add_password('realm', 'host', 'username', 'password')
opener = urllib.request.build_opener(proxy_handler, proxy_auth_handler)
with opener.open('http://www.example.com/login.html') as f:
    pass
```
urllib提供的功能就是利用程序去执行各种HTTP请求。如果要模拟浏览器完成特定功能，需要把请求伪装成浏览器。伪装的方法是先监控浏览器发出的请求，再根据浏览器的请求头来伪装，User-Agent头就是用来标识浏览器的。





参考资料：[廖雪峰Python3教程](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)