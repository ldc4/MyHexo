title: Synchronized原理
date: 2016-04-29 09:12:42
categories:
- JAVA
tags:
- jvm
- synchronized
- lock
- concurrent
---
以前一直对synchronized很是畏惧，而且在java web开发中，我并没有遇到需要并发的场景，事务会比较多用，但是也都是封装好了的。
甚至synchronized这个词都不能熟练的打出来，借着准备面试问题的机会，遇到了`synchronized与lock的区别`这个问题，就此在网上收集了些资料学习并整理。

<!--more-->

## synchronized与lock的区别

>1.ReentrantLock拥有Synchronized相同的并发性和内存语义，此外还多了锁投票，定时锁等候和中断锁等候

先打开JDK看一下：

ReentrantLock以前根本没用到过，甚至连它所在的包java.utils.concurrent.locks以及java.utils.concurrent都没有用过。
当然，知道有ConcurrentHashMap还是知道的。

“java.util.concurrent中所有类的方法及其子包扩展了这些对更高级别同步的保证。”
这句话表明了博文上的观点。

ReentrantLock获取锁定与三种方式：
```
lock()
如果获取了锁立即返回，如果别的线程持有锁，当前线程则一直处于休眠状态，直到获取锁
tryLock()
如果获取了锁立即返回true，如果别的线程正持有锁，立即返回false；
tryLock(long timeout, TimeUnit unit)
如果获取了锁定立即返回true，如果别的线程正持有锁，会等待参数给定的时间，在等待的过程中，如果获取了锁定，就返回true，如果等待超时，返回false；
lockInterruptibly()
如果获取了锁定立即返回，如果没有获取锁定，当前线程处于休眠状态，直到获取锁定，或者当前线程被别的线程中断
```

>2.synchronized是在JVM层面上实现的，不但可以通过一些监控工具监控synchronized的锁定，而且在代码执行时出现异常，JVM会自动释放锁定，但是使用lock则不行，lock是通过代码实现的，要保证锁定一定会被释放，就必须将unlock()放在finally{}中  

>3.在资源竞争不是很激烈的情况下，Synchronized的性能要优于ReetrantLock，但是在资源竞争很激烈的情况下，Synchronized的性能会下降几十倍，但是ReetrantLock的性能能维持常态；

## synchronized的三种使用模式

1. 作为修饰符加在方法声明上，synchronized修饰非静态方法时表示锁住了调用该方法的堆对象，修饰静态方法时表示锁住了这个类在方法区中的类对象（记住JAVA中everything is object）
2. 可以用synchronized直接构建代码块
3. 在使用Object.wait()使当前线程进入该Object的阻塞队列时，以及用Object.notify()或Object.notifyAll()唤醒该Object的阻塞队列中一个或所有线程时，必须在外层使用synchronized (Object)，这是JAVA中线程同步的最常见做法。

之所以在这里要强制使用synchronized代码块，是因为在JAVA语义中，wait有出让Object锁的语义，要想出让锁，前提是要先获得锁，所以要先用synchronized获得锁之后才能调用wait，notify原因类似，另外，我们知道操作系统信号量的增减都是原子性的，而Object.wait()和notify()不具有原子性语义，所以必须用synchronized保证线程安全。

## 使用synchronized的三个原则

1. sychronized的对象最好选择引用不会变化的对象
2. 尽可能把synchronized范围缩小
3. 尽量不要在可变引用上wait()和notify()

## synchronized与lock的使用场景

1.在业务并发简单清晰的情况下推荐synchronized，在业务逻辑并发复杂，或对使用锁的扩展性要求较高时，推荐使用ReentrantLock这类锁。
2.优先考虑synchronized，如果有特殊需要，再进一步优化，ReentrantLock和Atomic如果用的不好，不仅不能提高性能，还可能带来灾难

## 两种基本的锁实现（阻塞锁和自旋锁）

### 阻塞锁
我们平时说的锁都是通过阻塞线程来实现的：当出现锁竞争时，只有获得锁的线程能够继续执行，竞争失败的线程会由running状态进入blocking状态，并被登记在目标锁相关的一个等待队列中，当前一个线程退出临界区，释放锁后，会将等待队列中的一个阻塞线程唤醒（按FIFO原则唤醒），令其重新参与到锁竞争中。

**公平锁与非公平锁**
这里要区别一下公平锁和非公平锁，顾名思义，公平锁就是获得锁的顺序按照先到先得的原则，从实现上说，要求当一个线程竞争某个对象锁时，只要这个锁的等待队列非空，就必须把这个线程阻塞并塞入队尾（插入队尾一般通过一个CAS保持插入过程中没有锁释放）。相对的，非公平锁场景下，每个线程都先要竞争锁，在竞争失败或当前已被加锁的前提下才会被塞入等待队列，在这种实现下，后到的线程有可能无需进入等待队列直接竞争到锁。

非公平锁虽然可能导致活锁（所谓的饥饿），但是锁的吞吐率是公平锁的5-10倍，synchronized是一个典型的非公平锁，无法通过配置或其他手段将synchronized变为公平锁，在JDK1.5后，提供了一个ReentrantLock可以代替synchronized实现阻塞锁，并且可以选择公平还是非公平。
### 自旋锁
线程的阻塞和唤醒需要CPU从用户态转为核心态，频繁的阻塞和唤醒对CPU来说是一件负担很重的工作。同时我们可以发现，很多对象锁的锁定状态只会持续很短的一段时间，例如整数的自加操作，在很短的时间内阻塞并唤醒线程显然不值得，为此引入了自旋锁。

所谓“自旋”，就是让线程去执行一个无意义的循环，循环结束后再去重新竞争锁，如果竞争不到继续循环，循环过程中线程会一直处于running状态，但是基于JVM的线程调度，会出让时间片，所以其他线程依旧有申请锁和释放锁的机会。

自旋锁省去了阻塞锁的时间空间（队列的维护等）开销，但是长时间自旋就变成了“忙式等待”，忙式等待显然还不如阻塞锁。所以自旋的次数一般控制在一个范围内，例如10,100等，在超出这个范围后，自旋锁会升级为阻塞锁。

所谓自适应自旋锁，是通过JVM在运行时收集的统计信息，动态调整自旋锁的自旋上界，使锁的整体代价达到最优。

## synchronized原理

synchronized实现同步是利用了锁，synchronized锁在运行过程中可能经过N次升级变化
如：
**自适应自旋锁—>阻塞锁**
自适应自旋锁是JDK1.6中引入的，自旋锁的自旋上界由同一个锁上的自旋时间统计和锁的持有者状态共同决定。当自旋超过上界后，自旋锁就升级为阻塞锁。就像C中的Mutex，阻塞锁的空间和时间开销都比较大（毕竟有个队列），为此在阻塞锁中，synchronized又进一步进行了优化细分。

**偏向锁—>轻量锁—>重量锁**

### JVM中对象的内存布局

JVM中每个对象都有一个对象头（Object header），普通对象头的长度为两个字，数组对象头的长度为三个字（JVM内存字长等于虚拟机位数，32位虚拟机即32位一字，64位亦然），其构成如下所示：
![](http://ldc4.qiniudn.com/images/Synchronized/Synchronized-1.png "JAVA对象头结构")
ClassMetadataAddress是指向方法区中对象所属类对象的地址指针，ArrayLength标志了数组长度，MarkWord用于存储对象的各种标志信息，为了在极小的空间存储尽量多的信息，MarkWord会根据对象状态复用空间。MarkWord中有2位用于标志对象状态，在不同状态下MarkWord中存储的信息含义分别为：
![](http://ldc4.qiniudn.com/images/Synchronized/Synchronized-2.png "MarkWord结构")

### 轻量锁

首先需要明确的是，无论是轻量锁还是偏向锁，都不能代替重量锁，两者的本意都是在没有多线程竞争的前提下，减少重量锁产生的性能消耗。一旦出现了多线程竞争锁，轻量锁和偏向锁都会立即升级为重量锁。进一步讲，轻量锁和偏向锁都是重量锁的乐观并发优化。

**对对象加轻量锁的条件是该对象当前没有被任何其他线程锁住。**

先从对象O没有锁竞争的状态说起，这时候MarkWord中Tag状态为01，其他位分别记录了对象的hashcode，4位的对象年龄信息（新建对象年龄为0，之后每次在新生代拷贝一次就年龄+1，当年龄超过一个阈值之后，就会被丢入老年代，GC原理不是本文的主题，但至少我们现在知道了，这个阈值<=15），以及1位的偏向信息用于记录这个对象是否可用偏向锁。 当一个线程A在对象O上申请锁时，它首先检查对象O的Tag，若发现是01且偏向信息为0，表明当前对象还未加锁，或加过偏向锁（加过，注意是加过偏向锁的对象只能被同样的线程加锁，如果不同的线程想要获取锁，需要先将偏向锁升级为轻量锁，稍后会讲到），在判断对当前对象确实没有被任何其他线程锁住后（Tag为01或偏向线程不具有该对象锁），即可以在该对象上加轻量锁。

在判断可以加轻量锁之后，加轻量锁的过程为两步：

1.在当前线程的栈（stack frame）中生成一个锁记录（lock record），这个锁记录并不是我们通常意义上说的锁对象（包含队列的那个），而仅仅是对象头MarkValue的一个拷贝，官方称之为displayed mark value。如图所示：
![](http://ldc4.qiniudn.com/images/Synchronized/Synchronized-3.png "加轻量锁之前")

ps:个人理解：MarkValue是指的MarkWord除去Tag的Bitfield部分

2.通过CAS操作将上一步生成的lock record地址赋给目标对象的MarkValue中（Tag同时改为00）,保证在给MarkValue赋值时Tag不会动态修改，如果CAS成功，表明轻量锁申请成果，如果CAS不成功，且Tag变为00，则查看MarkValue中lock record address是否指向当前线程栈中的锁记录，若是，则表明是同样的线程锁重入，也算锁申请成果。如图所示： 
![](http://ldc4.qiniudn.com/images/Synchronized/Synchronized-4.png "加轻量锁之后")
在第二步中，若不满足加锁成功的两种情况，说明目标锁已经被其他线程持有，这时不再满足加轻量锁条件，需要将当前对象上的锁状态升级为重量锁：将Tag状态改为10，并生成一个Monitor对象（重量锁对象），再将MarkValue值改为该Monitor对象的地址。最后将当前线程塞入该Monitor的等待队列中。

轻量锁的解锁过程也依赖CAS操作（CAS，后面有讲解）： 通过CAS将lock record中的Object原MarkValue赋还给Object的MarkValue，若替换成功，则解锁完成，若替换不成功，表示在当前线程持有锁的这段时间内，其他线程也竞争过锁，并且发生了锁升级为重量锁，这时需要去Monitor的等待队列中唤醒一个线程去重新竞争锁。

当发生锁重入时，会对一个Object在线程栈中生成多个lock record，每当退出一个synchronized代码块便解锁一次，并弹出一个lock record。

<font color=red>**一言以蔽之，轻量锁通过CAS检测锁冲突，在没有锁冲突的前提下，避免采用重量锁的一种优化手段。**</font>

加轻量锁的代价是数个指令外加一个CAS操作，虽然轻量锁的代价已经足够小，它依然有优化空间。 细心的人应该发现，轻量锁的每次锁重入都要进行一次CAS操作，而这个操作是可以避免的，这便是偏向锁的优化手段了。


### 偏向锁

所谓偏向，就是偏袒的意思，偏向锁的初衷是在某个线程获得锁之后，消除这个线程锁重入（CAS）的开销，看起来让这个线程得到了偏护。

偏向锁和轻量锁的加锁过程很类似，不同的是在第二步CAS中，set的值是申请锁的线程ID，Tag置为01（就初始状态来说，是不变），这点可以从图2中看出。当发生锁重入时，只需要检查MarkValue中的ThreadID是否与当前线程ID相同即可，相同即可直接重入，不相同说明有不同线程竞争锁，这时候要先将偏向锁撤销（revoke）为轻量锁，再升级为重量锁。 因为偏向锁的MarkValue为线程ID，可以直接定位到持有锁的线程，偏向锁撤销为轻量锁的过程，需要将持有锁的线程中与目标对象相关的最老的lock record地址替换到当前的MarkValue中，并将Tag置为00。

偏向锁的释放不需要做任何事情，这也就意味着加过偏向锁的MarkValue会一直保留偏向锁的状态，因此即便同一个线程持续不断地加锁解锁，也是没有开销的。 另一方面，偏向锁比轻量锁更容易被终结，轻量锁是在有锁竞争出现时升级为重量锁，而一般偏向锁是在有不同线程申请锁时升级为轻量锁，这也就意味着假如一个对象先被线程1加锁解锁，再被线程2加锁解锁，这过程中没有锁冲突，也一样会发生偏向锁失效，不同的是这回要先退化为无锁的状态，再加轻量锁，如图：
![](http://ldc4.qiniudn.com/images/Synchronized/Synchronized-5.png "偏向锁，以及锁升级")

回到图2，我们发现出了Tag外还有一个01标志位，上文中提到，这位表示偏向信息，0表示偏向不可用，1表示偏向可用，这位信息同样记录在对象的类对象中，当JVM发现一类对象频繁发生锁升级，而锁升级本身需要一定的开销，这种情况下偏向锁反而成为一种负担，尤其在生产者消费者这类常态竞争锁的场景中，偏向锁是完全无意义的，当JVM搜集到足够的“证据”证明偏向锁不应当存在后，它就会将类对象中的相关标志置0，之后每次生成新对象其偏向信息都是0，都不会再加偏向锁。官网上称之为Bulk revokation。

另外，JVM对那种会有多线程加锁，但不存在锁竞争的情况也做了优化，听起来比较拗口，但在现实应用中确实是可能出现这种情况，因为线程之前除了互斥之外也可能发生同步关系，被同步的两个线程（一前一后）对共享对象锁的竞争很可能是没有冲突的。对这种情况，JVM用一个epoch表示一个偏向锁的时间戳（真实地生成一个时间戳代价还是蛮大的，因此这里应当理解为一种类似时间戳的identifier），对epoch，官方是这么解释的：

A similar mechanism, called bulk rebiasing, optimizes situations in which objects of a class are locked and unlocked by different threads but never concurrently. It invalidates the bias of all instances of a class without disabling biased locking. An epoch value in the class acts as a timestamp that indicates the validity of the bias. This value is copied into the header word upon object allocation. Bulk rebiasing can then efficiently be implemented as an increment of the epoch in the appropriate class. The next time an instance of this class is going to be locked, the code detects a different value in the header word and rebiases the object towards the current thread.

<font color=red>**再次一言以蔽之，偏向锁是在轻量锁的基础上减少了锁重入的开销。**</font>

### 重量锁
重量锁在JVM中又叫对象监视器（Monitor），它很像C中的Mutex，除了具备Mutex互斥的功能，它还负责实现了Semaphore的功能，也就是说它至少包含一个竞争锁的队列，和一个信号阻塞队列（wait队列），前者负责做互斥，后一个用于做线程同步。

（重量锁略,以后再补充）

这里还需要说明一下自旋锁与阻塞锁三个过程之间的关系：自旋锁是在发生锁竞争时自旋等待，那么自旋锁的前提是发生锁竞争，而轻量锁，偏向锁的前提都是没有锁竞争，所以加自旋锁应当发生在加重量锁之前，准确地说，是在线程进入Monitor等待队列之前，先自旋一会，重新竞争，如果还竞争不到，才会进入Monitor等待队列。加锁顺序为：

偏向锁—>轻量锁—>自适应自旋锁—>重量锁

### Java CAS

独占锁是一种悲观锁，synchronized就是一种独占锁，会导致其它所有需要锁的线程挂起，等待持有锁的线程释放锁。而另一个更加有效的锁就是乐观锁。所谓乐观锁就是，每次不加锁而是假设没有冲突而去完成某项操作，如果因为冲突失败就重试，直到成功为止。

CAS 操作

上面的乐观锁用到的机制就是CAS，Compare and Swap。

CAS有3个操作数，内存值V，旧的预期值A，要修改的新值B。当且仅当预期值A和内存值V相同时，将内存值V修改为B，否则什么都不做。

非阻塞算法 （nonblocking algorithms）

一个线程的失败或者挂起不应该影响其他线程的失败或挂起的算法。

现代的CPU提供了特殊的指令，可以自动更新共享数据，而且能够检测到其他线程的干扰，而 compareAndSet() 就用这些代替了锁定。

拿出AtomicInteger来研究在没有锁的情况下是如何做到数据正确性的。

private volatile int value;

首先毫无以为，在没有锁的机制下可能需要借助volatile原语，保证线程间的数据是可见的（共享的）。

这样才获取变量的值的时候才能直接读取。
```
public final int get() {
        return value;
    }
```
然后来看看++i是怎么做到的。
```
public final int incrementAndGet() {
    for (;;) {
        int current = get();
        int next = current + 1;
        if (compareAndSet(current, next))
            return next;
    }
}
```
在这里采用了CAS操作，每次从内存中读取数据然后将此数据和+1后的结果进行CAS操作，如果成功就返回结果，否则重试直到成功为止。

而compareAndSet利用JNI来完成CPU指令的操作。
```
public final boolean compareAndSet(int expect, int update) {   
    return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
}
```
整体的过程就是这样子的，利用CPU的CAS指令，同时借助JNI来完成Java的非阻塞算法。其它原子操作都是利用类似的特性完成的。

而整个J.U.C(java.util.concurrent包)都是建立在CAS之上的，因此对于synchronized阻塞算法，J.U.C在性能上有了很大的提升。参考资料的文章中介绍了如果利用CAS构建非阻塞计数器、队列等数据结构。

CAS看起来很爽，但是会导致“ABA问题”。

CAS算法实现一个重要前提需要取出内存中某时刻的数据，而在下时刻比较并替换，那么在这个时间差类会导致数据的变化。

比如说一个线程one从内存位置V中取出A，这时候另一个线程two也从内存中取出A，并且two进行了一些操作变成了B，然后two又将V位置的数据变成A，这时候线程one进行CAS操作发现内存中仍然是A，然后one操作成功。尽管线程one的CAS操作成功，但是不代表这个过程就是没有问题的。如果链表的头在变化了两次后恢复了原值，但是不代表链表就没有变化。因此前面提到的原子操作AtomicStampedReference/AtomicMarkableReference就很有用了。这允许一对变化的元素进行原子操作。


## 参考资料

[Lock与synchronized 的区别](http://houlinyan.iteye.com/blog/1112535)
`http://houlinyan.iteye.com/blog/1112535`
[JVM锁实现探究1：synchronized初探](http://www.majin163.com/2014/03/17/synchronized1/)
`http://www.majin163.com/2014/03/17/synchronized1/`
[JVM锁实现探究2：synchronized深探](http://www.majin163.com/2014/03/17/synchronized2/)
`http://www.majin163.com/2014/03/17/synchronized2/`
[深入浅出 Java Concurrency (5): 原子操作 part 4](http://www.blogjava.net/xylz/archive/2010/07/04/325206.html)
`http://www.blogjava.net/xylz/archive/2010/07/04/325206.html`