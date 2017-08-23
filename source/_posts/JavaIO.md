title: Java编程思想之Java I/O系统
date: 2016-04-22 20:58:11
categories:
- JAVA
tags:
- io
- j2se
---

对于阿里笔试附加题，我是完全做不来，第一题就考察的是io，然后我就回头看了看编程思想上的I/O系统，然后简要记录一下笔记。
这种需要大量记忆能力的东西，我发现我现在只有被打脸，愧叹记忆不如当年，当年的记忆好，但是没有意识啊。

<!--more-->
# File类
File类能代表一个特定的文件的名称，也能代表一个目录下的一组文件的名称。

如果它指的文件集，可以对此集合调用list()

## 目录过滤器

FilenameFilter接口

正则小插曲： *表示匹配子表达式0次或多次

## 目录实用工具

深度遍历目录，区分文件和目录。

以及制定文件进行某种处理

## 目录的检查及创建

File对象可以用来创建新的目录或尚不存在的整个目录路径。
还可以查看文件的特性（如：大小，最后修改日期，读/写），
检查某个File对象代表的是一个文件还是一个目录，并且可以删除文件。

具体请查看File API


# 输入和输出
InputStream的作用是用来表示那些从不同数据源产生输入的类

类型：
ByteArray
StringBuffer
File
Piped
Sequence
Filter(装饰)

OutputStream的作用决定输出所要去往的目标

类型：
ByteArray 内存缓冲区
File
Piped
Filter


# 添加属性和有用的接口

FilterInputStream类在内部修改InputStream的行为方式：
是否缓冲，是否保留它所读过的行，以及是否把单一字符推回输入流
```
FilterInputStream
    |- DataInputStream
    |- BufferedInputStream
    |- LineNumberInputStream
    |- PushbackInputStream

FilterOutputStream
    |- DataOutputStream
    |- PrintStream
    |- BufferedOutputStream
```
PrintStream最初的目的便是为了以可视化格式打印所有的基本数据类型以及String对象
DataOutputStream却不同，是将数据元素置入”流“中，是DataInputStream能够可移植地重构它们


# Reader和Writer

InputStreamReader可以将InputStream转换成Reader

Java 1.1引进

# RandomAccessFile

在JDK1.4中，RandomAccessFile的大多数功能由nio存储映射文件所取代。

# I/O流的典型使用方式

## 缓冲输入文件
```
BufferedReader in = new BufferedReader(new FileReader(file));
String s;
StringBuilder sb = new StringBuilder();
while((s=in.readLine())!=null){
     sb.append(s+"\n");
}
```
## 从内存输入
```
StringReader in = new StringReader("ldc4afajkjflsjf");
int c;
while((c=in.read())!=-1){
     System.out.print((char)c);
}
```
## 格式化的内存输入
```
DataInputStream in = new DataInputStream(new ByteArrayInputStream("ldc4".getBytes()));
while(in.available() != 0){
     System.out.print((char)in.readByte());
}
```
## 基本的文件输出
```
BufferedReader in = new BufferedReader(new StringReader("ldc4"));
PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter(file)));
//java se5添加了辅助构造器，可以直接new PrintWriter(file)
String s;
while((s=in.readLine())!=null){
     out.println(s);
}
out.close();//如果我们不为所有的输出文件调用close()，就会发现缓冲区内容不会被刷新清空，那么它们也就不完整
```
## 存储和恢复数据

如果我们使用DataInputStream写入数据，Java保证我们可以使用DataOutputStream准确地读取数据

## 读写随机访问文件
```
RandomAccessFile rf = new RandomAccessFile(file,"rw");
rf.seek(5*8);
rf.writerDouble(47.0001)；
rf.close();
```

## 管道流

PipedInputStream、PipedOutputStream、PipedReader、PipedWriter

# 文件读写的使用工具
...

# 标准I/O
System.in     InputStream
System.out  PrintStream
System.err   PrintStream

System.setIn(in);
System.setOut(out);
System.setErr(err);

java.util.Sanner

# 进程控制
Process
ProcessBuilder
process.getInputStream

# NIO(新IO，同步非阻塞IO)

JDK1.4的java.nio.*包中引入了新的JavaI/O类库，其目的在于提高速度

旧的IO包已经使用nio重新实现过。

速度的提高来自于所使用的结构更接近于操作系统执行I/O的方式：通道和缓冲器


唯一与通道交互的缓冲区：ByteBuffer

ByteBuffer.wrap("ldc4".getBytes())
ByteBuffer.allocate(1024);

FileChannel fc =new XXX(流).getChannel();
fc.write(buff)

buff.flip() // Prepare for writing
buff.clear()//Prepare for reading

transferTo和transferFrom用于连接通道

in.transferTo(0,in.size(),out);
out.transferFrom(in,0,in.size());

## 转换数据

buff.rewind
感觉回到了C语言的读写文件。。

CharBuffer

Charset.forName(System.getProperty("file.encoding")).decode(buff)

buff.asCharBuffer()

## 获取基本类型

asIntBuffer
asLongBuffer
asFloatBuffer
...


## 视图缓冲区

可以让我们通过某个特定的基本数据类型分视窗查看其底层的ByteBuffer

## 用缓冲器操纵数据

ByteBuffer是将数据移进移出通道的唯一方法

![](http://ldc4.qiniudn.com/images/JavaIO/JavaIO-1.png)

## 缓冲器的细节

Buffer由数据和可以高效地访问及操纵这些数据的四个索引组成

四个索引：mark(标记)、position(位置)、limit(界限)和capacity(容量)

capacity() 返回缓冲区容量
limit() 返回limit值
limit(int lim) 设置limit值
position() 返回position值
position(int pos) 设置position值
clear() 清空缓冲区，position = 0，limit = capacity
flip()    limit = position , position = 0
mark() mark = position
reamain() 返回 limit - position

## 内存映射文件

MappedByteBuffer

映射文件中的所有输出必须使用RandomAccessFile

## 文件加锁

FlieLock

FileChannel调用tryLock()或lock()

SocketChannel、DatagramChannel和ServerSocketChannel不需要加锁，因为他们是从单进程实体继承而来
我们通常不在两个进程间共享网络socket

tryLock是非阻塞式的
lock是阻塞式的

对部分文件加锁：
tryLock(long position, long size, boolean shared)
lock(long position, long size, boolean shared)
