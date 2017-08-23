title: LeetCode No.127 Word Ladder
categories:
  - Arithmetic
tags:
  - leetcode
  - BFS
date: 2016-03-14 10:08:12
---
翻译：
给出两个单词（start和end）和一个字典，找到从start到end的最短转换序列
比如：
1.每次只能改变一个字母。
2.变换过程中的中间单词必须在字典中出现。

start = "hit"
end = "cog"
dict = ["hot","dot","dog","lot","log"]
一个最短的变换序列是"hit"->"hot" -> "dot"-> "dog"->"cog"
返回它的长度 5

注意:
- 如果没有转换序列则返回0。
- 所有单词具有相同的长度。
- 所有单词都只包含小写字母。
<!--more-->

自白：
刚接触这题，自己凭着水题一般直接暴力解，出了很多问题。然后查看discuss，学到了BFS算法。

### BFS抽象代码
``` c
bool BFS(Node& Vs, Node& Vd, ...){
	queue<Node> Q;//队列，用于存放待搜索节点
	Node Vn, Vw;//搜索节点，到达节点

	//初始状态将起点放进队列Q
	Q.push(Vs);
	hash(Vs) = true;//设置节点已经访问过了！

	while (!Q.empty()){//队列不为空，继续搜索！
		//取出队列的头Vn
		Vn = Q.front();

		//从队列中移除
		Q.pop();

		while(Vw = Vn通过某规则能够到达的节点){
			if (Vw == Vd){//找到终点了！
				//把路径记录，这里没给出解法
				return true;//返回
			}

			if (isValid(Vw) && !visit[Vw]){
				//Vw是一个合法的节点并且为白色节点
				Q.push(Vw);//加入队列Q
				hash(Vw) = true;//设置节点颜色
			}
		}
	}
	return false;//无解
}
```

### LeetCode No.127 submit
利用了BFS算法。
这里直接是把访问过的删掉，不用标记了，可以提高点效率。
``` java
public int ladderLength(String beginWord, String endWord, Set<String> wordList) {
	Queue<String> queue = new LinkedList<String>();
	queue.add(beginWord);

	int level=1;//设置计数器

	while(!queue.isEmpty()){
		int size = queue.size();//别直接把queue.size()直接放在循环中，size()也是要时间的。
		level++;
		for(int k=0;k<size;k++){
			String s = queue.poll();
			char[] cs = s.toCharArray();
			int length = s.length();
			for(int i=0;i<length;i++){
				char t = cs[i];//暂存原本的字符
				for(int j=0;j<26;j++){
					cs[i] = (char)(j+'a');
					String str = new String(cs);
					if(endWord.equals(str)){
						return level;
					}
					if(wordList.contains(str)){
						queue.add(str);
						wordList.remove(str);
						continue;
					}
				}
				cs[i] = t;//还原字符串
			}
		}	
	}
	return 0;
}
```

### 总结
值得注意的一点，我之前Sb的用了栈结构，调试的时候，发现栈里面不断在进内容，而栈底的节点本应该是第二层的，有比它更高的层数都执行了。

还有就是这里直接删除，可以提高效率，少了不少遍历，continue到是没有太明显的效果，可要可不要。
而且这里删除操作不能在遍历的时候删。操作集合的时候要特别小心这些。

更高效的解决算法：双端BFS。 等之后再回头来看看

[参考网站](http://rapheal.iteye.com/blog/1526861)


### 做题心理过程
>
读完题的第一想法：

想要找到转换序列，就得倒推
设置一个变量来记录序列的长度（“序列“对象包含”长度“）len=1，
从cog 开始 ，去找 ?og,c?g,co?，没找到表示没有
找到，dog,log , len+=1,和start比较，如果得到start就OK，否则
又以dog和log开始找，重复过程。这里显然是个递归

最后在所有转换序列中找到最短的。  思考：这个最短的是否可以在递归的过程中就确定




※如何在字典中查找匹配字符串？？？

把字符串”abcd“拆分成4条正则表达式？
?bcd a?cd ab?d abc?


遇到问题：在查找时找到了，然后递归的时候又匹配到原来的单词  ?og找到dog,然后拿dog得到的？og又得到dog

去掉dog    ["hot","dot","dog","lot","log"] => ["hot","dot","lot","log"]

?og 还是会匹配到 log,而我又管不到？og的生成。 

虽然匹配到了log，但是下一步就会直接去掉log，似乎没影响，就是 消耗的资源变多了，效率有点低


cog -> dog -> log ->lot->hot->dot

出现一个异常
```
Exception in thread "main" java.util.ConcurrentModificationException
     at java.util.HashMap$HashIterator.nextEntry(Unknown Source)
     at java.util.HashMap$KeyIterator.next(Unknown Source)
     at net.ldc4.argorithm.leetcode.NO127.Solution.findWord(Solution.java:52)
     at net.ldc4.argorithm.leetcode.NO127.Solution.findWord(Solution.java:66)
     at net.ldc4.argorithm.leetcode.NO127.Solution.findWord(Solution.java:66)
     at net.ldc4.argorithm.leetcode.NO127.Solution.findWord(Solution.java:66)
     at net.ldc4.argorithm.leetcode.NO127.Main.main(Main.java:22 )

52               String str = it.next();
```
addAll方法：
如果 set 中没有指定 collection 中的所有元素，则将其添加到此 set 中（可选操作）。如果指定的 collection 也是一个 set，则 addAll 操作会实际修改此 set，这样其值是两个 set 的一个并集。如果操作正在进行的同时修改了指定的 collection，则此操作的行为是不确定的。
```
Set<String> dictClone = new HashSet<String>();
dictClone.addAll(dict);
```
这样clone是错误的！！！
```
Set<String> dictClone = new HashSet<String>();
Iterator<String> it = dict.iterator();
while( it.hasNext()){
     dictClone.add(it.next());
}
```
这样还是错。。。

TT

测试了一下
之前那样写和后面这种结果是一样的，异常原因应该不在这
```
                                          |
                                          v
i=0   ?og   -> dictClone ->["hot","dot","dog","lot","log"]->["hot","dot","lot","log"]
i=1   c?g
i=2   co?
```
思路完全混乱了。。

![](http://ldc4.qiniudn.com/images/LeetCode127/leetcode127-1.png)

而且画图的时候 发现，最后匹配beginWord也有问题

果然还是太弱了 我。

去看看别人的思路

2016年3月13日19:45:01

重新回来看一下这个题

Discuss 里面 有用BFS来实现

百度了一下 原来是广度/宽度优先搜索，以前数据结构学过吧，记得当时自己还实现了的，现在忘得一干二净了

” 很多最短路径算法就是基于广度优先的思想成立的。“

![](http://ldc4.qiniudn.com/images/LeetCode127/leetcode127-2.png)
![](http://ldc4.qiniudn.com/images/LeetCode127/leetcode127-3.png)

wa3次 第一次不应该，变量名没改

第二次for循环直接用的3... 应该用String转换成char[]的长度
第三次TLE了。。。   效率太低。。 看还能优化不



最后改成和参考算法一样了。


不过还有更快的算法，双端BFS

啊啊啊啊，不深究了。 之后在看吧
>


源代码，出门右转：[github/LeetCode](https://github.com/ldc4/LeetCode)