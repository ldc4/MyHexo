title: LeetCode No.133 Clone Graph
date: 2016-04-28 13:48:30
categories:
- Arithmetic
tags:
- leetcode
- graph
- dfs
- bfs
---

Clone an undirected graph. Each node in the graph contains a label and a list of its neighbors.

OJ's undirected graph serialization:Nodes are labeled uniquely.
We use # as a separator for each node, and , as a separator for node label and each neighbor of the node.
As an example, consider the serialized graph {0,1,2#1,2#2,2}.
The graph has a total of three nodes, and therefore contains three parts as separated by #.

	1. First node is labeled as 0. Connect node 0 to both nodes 1 and 2.
	2. Second node is labeled as 1. Connect node 1 to node 2.
	3. Third node is labeled as 2. Connect node 2 to node 2 (itself), thus forming a self-cycle.


Visually, the graph looks like the following:
```
       1
      / \
     /   \
    0 --- 2
         / \
         \_/

```

<!--more-->


## 思路
看了半天题，发现下面那堆根本没什么用。而且我还觉得它那个序列是不是有问题。
先不管那个，这个题就是让你遍历图然后复制一个图出来。

因为之前也做过BFS，想直接上去撸代码，发现，给的数据结构没有mark值，这咋办呢？

hashmap

当我写cloneNode的时候，发现直接就复制完成了，但是一想不对，那样只是复制引用。
没有达到正真的clone

第一遍写完后运行，死循环。原因：在遍历后没有上色

跟着遍历来创建图的话，你的每个节点都是new的这样不对。

必须得换一种思路：就像题意那样，先把图遍历成一个序列来表示，然后再用这个序列来重构图。

偷偷看看discuss

DFS DFS DFS...

明明这么简单的事情为什么要想得那么复杂。

AC，虽然又用的别人的代码。

但是仔细一想BFS按理也行吧，于是回头改改自己之前写的BFS

Input:{0,0,0}
Output:{0,0,0#0#0}
Expected:{0,0,0}

于是出现了如上WA

0,0,0是什么意思。。不是label唯一么

发现代码少写了：
map.put(result.label, result);


Input:{0,1#1,2#2,2}
Output:{0,1#1,2#2,2#2}
Expected:{0,1#1,2#2,2}


但是还是WA

我算是终于看懂了{0,1#1,2#2,2}是个什么意思了。。。

我之前看成，逗号分隔的了。。
0
1#1
2#2
2

其实应该这样看：
0,1 表示 0和1相连
1,2 表示 1和2相连
2,2 表示 2自环


map.put写错位置了 应该放在cloneNode函数中

Input:{0,0,0}
Output:{0,0,0#0}
Expected:{0,0,0}


然而还是WA了

单节点的自循环会有问题

特例处理（第一个节点的自循环）
但是怎么判断它是第一次进入呢

。。。


卧槽，问题想明白了一下就能解决，
为什么为出现第一个节点的自循环有问题？
因为由于第一个节点是新建的，没有加入到map中，自循环都是在已有节点的基础上再操作的。
怎么改？
在一开始创建第一个节点的时候就map.put
map.put(result.label, result); //!!!
而不是让它去map中搜索，第一个肯定是没有的嘛！
那样就创建了一个新节点了。


恩,AC了。14ms效率的确比DFS（4ms）低


## AC代码（DFS）
```
package net.ldc4.argorithm.leetcode.NO133;

import java.util.HashMap;


public class Solution3 {
	// discuss,这种依赖于label唯一
	HashMap<Integer,UndirectedGraphNode> map = new HashMap<Integer,UndirectedGraphNode>();
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
    	
    	if(node == null)
    		return null;
    	
    	UndirectedGraphNode res = new UndirectedGraphNode(node.label);
    	map.put(node.label, res);
    	for(UndirectedGraphNode n: node.neighbors){
    		UndirectedGraphNode nb = map.get(n.label);
    		if(nb == null){
    			nb = cloneGraph(n);
    		}
    		res.neighbors.add(nb);
    	}
    	return res;
    }
}
```

## AC代码（BFS）
```
package net.ldc4.argorithm.leetcode.NO133;

import java.util.HashMap;
import java.util.Iterator;
import java.util.LinkedList;
import java.util.Queue;


/**
 * 有问题：跟着遍历来创建图的话，你的每个节点都是new的这样不对。
 * 已解决
 * @author ldc4
 */
public class Solution {
	
	private boolean flag = true;
	public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
		
		if(node == null)
			return null;
		
		Queue<UndirectedGraphNode> queue1 = new LinkedList<UndirectedGraphNode>();
		Queue<UndirectedGraphNode> queue2 = new LinkedList<UndirectedGraphNode>();
		HashMap<UndirectedGraphNode, Boolean> mark = new HashMap<UndirectedGraphNode, Boolean>();
		HashMap<Integer, UndirectedGraphNode> map = new HashMap<Integer, UndirectedGraphNode>();
		
		//DFS是基于栈的遍历
		queue1.add(node);
		//上色
		mark.put(node, true);
		UndirectedGraphNode result = new UndirectedGraphNode(node.label);
		map.put(result.label, result);//!!!
		cloneNode(node,result,map);
		queue2.add(result);
		
		//queue1和queue2始终保持一致
		while(!queue1.isEmpty()){
			int size = queue1.size();
			for(int i=0; i<size; i++){
				UndirectedGraphNode pollNode1 = queue1.poll();
				UndirectedGraphNode pollNode2 = queue2.poll();
				
				Iterator<UndirectedGraphNode> it1 = pollNode1.neighbors.iterator();
				Iterator<UndirectedGraphNode> it2 = pollNode2.neighbors.iterator();
				
				while(it1.hasNext()){
					UndirectedGraphNode nextNode1 = it1.next();
					UndirectedGraphNode nextNode2 = it2.next();
					if(mark.get(nextNode1)==null){
						//clone该节点
						cloneNode(nextNode1, nextNode2, map);
						
						queue1.offer(nextNode1);
						mark.put(nextNode1, true);
						queue2.offer(nextNode2);
					}
				}
			}
			
		}
		
		return result;
	}
	
	//克隆节点:node2 = node1;
	private void cloneNode(UndirectedGraphNode node1,UndirectedGraphNode node2,HashMap<Integer, UndirectedGraphNode> map){
		
		Iterator<UndirectedGraphNode> it = node1.neighbors.iterator();
		while(it.hasNext()){
			UndirectedGraphNode next = it.next();
			UndirectedGraphNode n = map.get(next.label);
			if(n == null){
				n = new UndirectedGraphNode(next.label);
				map.put(n.label, n);
			}
			node2.neighbors.add(n);
		}
	}
}
```

源代码，出门右转：[github/LeetCode](https://github.com/ldc4/LeetCode)