title: Shell基础语法
date: 2016-06-13 11:23:50
categories:
- Linux
tags:
- shell
---

鸟哥第一遍刷完了，就紧接着回顾了一下shell，
并没有看鸟哥书上的，网上随便找了个资源看了看。

<!--more-->

## 变量定义

变量名和等号之间不能有空格
首个字符必须为字母（a-z，A-Z）。
中间不能有空格，可以使用下划线（_）。
不能使用标点符号。
不能使用bash里的关键字（可用help命令查看保留关键字）。

例： `myname="ldc4"`

## 使用变量

**在变量前加上$符号**

变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界

例：
```
myname="ldc4"
echo $myname
echo ${myname}ismyname
```

## 只读变量
`readonly`修饰变量

修改自读变量会报错

## 删除变量
`unset variable_name`

## 变量类型
运行shell时，会同时存在三种变量：
1) 局部变量 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。

2) 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。

3) shell变量 shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

## shell字符串

字符串可以用单引号，也可以用双引号，也可以不用引号。
单引号里面的 $变量名 不被替换。
双引号里面的 $变量名 会被替换。且可以用转义字符

**拼接字符串：不需要+号，但不能留空格**

获取字符串长度：
```
var1="abcd"
echo ${#var1}
```
提取子字符串：
```
var2="i was sleepy"
echo ${var2:2:5}
```
## shell数组
bash支持一维数组（不支持多维数组），并且没有限定数组的大小。
类似与C语言，数组元素的下标由0开始编号。
获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于0。

定义数组：
数组名=(值1 值2 值3)

例：
`array_name=(value0 value1 value2 value3)`
或者
```
array_name=(
value0
value1
value2
value3
)
```
还可以单独定义数组的各个分量：
```
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```
可以不使用连续的下标，而且下标的范围没有限制。

读取数组：
${数组名[下标]}

例：
`value={array_name[0]}`

`*`,@可以获取所有数组元素
```
value={array_name[@]}
value={array_name[*]}
```
获取数组长度：
```
length=${#array_name[@]}
length=${#array_name[*]}
```
获取单个元素的长度：
```
length=${#array_name[0]}
```

## shell注释

以"#"开头的行就是注释，会被解释器忽略。
sh里没有多行注释，只能每一行加一个#号

如果在开发过程中，遇到大段的代码需要临时注释起来，过一会儿又取消注释，怎么办呢？
每一行加个#符号太费力了，可以把这一段要注释的代码用一对花括号括起来，定义成一个函数，没有地方调用这个函数，这块代码就不会执行，达到了和注释一样的效果。

## shell传递参数
我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。
n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……
其中 $0 为执行的文件名

还有几个特殊字符用处理参数：
**$$ $# $* $@ $! $? $-**

$* 与 $@ 区别：
相同点：都是引用所有参数。
不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。

## shell基本运算符
算数运算符：
+-*/% = == !=
原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。

例：
```
a=12
b=5
echo `expr $a + $b`
echo `expr $a \* $b`
```
表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
完整的表达式要被` ` ` ` ` ` 包含，注意这个字符不是常用的单引号，在 Esc 键下边。

乘号`(*)`前边必须加反斜杠`(\)`才能实现乘法运算；


关系运算符： 
```
-eq  (equal)
-ne  (not equal)
-gt   (greater than)
-lt    (less than)
-ge   (greater than or equal)
-le    (less than or equal)
```
关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

因为`<>`被用作数据流重定向了，所有没有用`><`来充当大于小于（个人理解）

布尔运算符:
!
-o
-a

逻辑运算符：
&&
||

注意这里是双中括号

例：
```
if [[ $a != $b && $a == $c ]]
then 
     echo xxx
fi
```
字符串运算符:
=
!=
-z zero
-n no zero
str (这里表示直接写字符串变量本身)

= 与 == 都能进行比较
不定义，定义一个空字符串，或者字符串为空格，都会被判定为空


文件测试运算符:
```
-b block
-c char
-d directory
-f file
-g SGID
-k Strick Bit
-p pipe
-u SUID
-r  read
-w write
-x execute
-s size
-e exist
```

## echo命令

显示普通字符串：
`echo I am ldc4,as weedust`

不加引号的字符串，可以含有空格

显示转义字符：
`echo \"hey, june\"`

显示变量：
```
read name
echo hello,$name
```
read 命令从标准输入中读取一行,并把输入行的每个字段的值指定给 shell 变量

显示换行：
`echo -e "haha\n"`

显示不换行：
`echo -e "haha\c"`

显示结果定向至文件：
`echo I am ldc4, as weedust > out.txt`

原样输出字符串，不进行转义或取变量（用单引号）：
`echo '$name\"'`

显示命令执行结果（用反引号）：
```
echo `date`
```
## printf命令
`printf  format-string  [arguments...]`

例：
```
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg 
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876
```
%s %c %d %f都是格式替代符
%-10s 指一个宽度为10个字符（-表示左对齐，没有则表示右对齐），任何字符都会被显示在10个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。
%-4.2f 指格式化为小数，其中.2指保留2位小数。

## test命令

test可以替换掉if的中括号`[]`

## shell流程控制
shell的流程控制不能为空，如果else分支没有语句执行，就不要写else

ifelse:
```
if [ xxx ]
then
    xxx
elif [ xxx ]
then
    xxx
else
    xxx
fi
```
如果要写成一行的话，得要添加;号

for:
```
for var in xxx
do
    xxx
done
```
while:
```
while (( xxx ))
do
    xxx
done
```

无限循环：
```
while :
do
     xxx
done
```
或
```
while true
do
     xxx
done
```


until:
```
until (( xxx ))
do
     xxx
done
```
**[ xxx ] 与 (( xxx )) 都能表示条件**

case:
```
case 值 in
模式1)
    xxx
    ;;
模式2）
    xxx
    ;;
esac
```
如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。

break 与 continue

## shell函数
```
[ function ] funname [()]
{
    action;
    [return int;]
}
```
中括号表示可要可不要
return 返回，如果不加，将以最后一条命令运行结果，作为返回值。
return后跟数值n(0-255)
返回值，在函数调用后用$?来获取

所有函数在使用前必须定义。这意味着必须将函数放在脚本开始部分，直至shell解释器首次发现它时，才可以使用。调用函数仅使用其函数名即可。

函数参数：
在函数体内部，通过 $n 的形式来获取参数的值
$10 不能获取第十个参数，获取第十个参数需要${10}
当n>=10时，需要使用${n}来获取参数。

## shell I/O重定向
```
> 输出重定向
< 输入重定向
>> 将输出以追加的方式重定向
>& 将输出文件合并
<& 将输入文件合并
```
文件描述符：
0 通常是标准输入（STDIN），
1 是标准输出（STDOUT），
2 是标准错误输出（STDERR）

例：
`command 2 > file`
`command > file 2>&1`

Here Document:`(<<)`

Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。
```
command << delimiter
    document
delimiter
```
/dev/null 可以起到”禁止输出“的作用

## shell文件包含

`. filename`
或
`source filename`

被包含的文件不需要可执行权限。

被包含的代码也会执行。


参考资料：
http://www.runoob.com/linux/linux-shell.html