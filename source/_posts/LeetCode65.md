title: LeetCode No.65 Ratote List
date: 2016-04-01 12:23:34
categories:
- Arithmetic
tags:
- leetcode
- list
---
Given a list, rotate the list to the right by k places, where k is non-negative.
For example:
Given 1->2->3->4->5->NULL and k = 2,
return 4->5->1->2->3->NULL.

Subscribe to see which companies asked this question

<!--more-->
这个题居然是medium,好吧我把medium想成最难的了。中等中等中等，英语差怪我咯

## 思路

根据例子来看，将4节点以后的元素挨个往1节点前插入。

边界：

	- k=0,没必要把其他节点插入到第一个之前，直接把第一个节点放到最后。
		这里延伸一个问题：对于靠前的K值，可以把前面的节点往后插。所以k=0不是边界
	- 只有一个元素或没有元素
	- k=length-1,即最后一个元素


①很有趣的事情是，这其实就是一个环，只是在不同的位置拆开而已。然而，我一步一步的操作过来了。。

②更有趣的事情是，k是从右边开始计算的。。

---

这里就称呼我的解法为暴力算法，先解决②问题，然后再优化。

如何找到链表右边第K个字符呢？

length - K。

so,加上
k = length - k - 1;

我觉得暴力算法会超时

wa

Input:[1,2] 3
Output:[1,2]
Expected:[2,1]


从结果就可以看出它的算法，就是用的环形链表，我这里的话必须得做模处理
k = k%length;

真是意外,居然ac了。。

经历千辛万苦啊！

意外的是居然才1ms

其实想明白了环操作，一会就写出来了。仅仅只用了几分钟


## AC代码

### 暴力链表操作
```
package net.ldc4.argorithm.leetcode.NO65;

public class Solution2 {

    public ListNode rotateRight(ListNode head, int k) {
        //计算长度,顺便记录尾节点
    	int length = 0;
    	
    	if(head==null){
    		return head;
    	}else{
    		length = 1;
    	}
    	
    	ListNode temp = head,tail = null;
    	while(temp.next!=null){
    		temp = temp.next;
    		length ++;
    	}
    	k = k%length;
    	if(length==1)
    		return head;
    	k = length - k - 1;
    	
    	tail = temp;
    	ListNode k1=head,k2 = null,k3 = null;
    	//判断移动前半段还是移动后半段
    	if(k>=(length-1)/2){
    		//移动后半段：找到k节点
    		for(int i=0;i<k;i++){
    			k1 = k1.next;
    		}
    		if(k1.next==null)
    			return head;
    		k2 = k1.next;//给k2赋值
    		k1.next = null;
    		k1 = k2.next;
    		k2.next = head;
    		head = k2;
    		while(k1!=null){
    			k3 = k2.next;
    			k2.next = k1;
    			k2 = k2.next;
    			k1 = k1.next;
    			k2.next = k3;
    		}
    		return head;
    	}else{
    		//移动前半段：找到尾节点（前面计算长度时记录了）
    		k2 = tail;
    		for(int i=0;i<=k;i++){
    			k2.next = k1;
    			k2 = k2.next;
    			k1 = k1.next;
    			k2.next = null;
    		}
    		return k1;
    	}
    }	
}
```

### 环形链表

```
package net.ldc4.argorithm.leetcode.NO65;

public class Solution3 {

	/*
	 * 思路，先弄成环，然后再打开
	 */
	public ListNode rotateRight(ListNode head, int k) {
		
		//计算长度,顺便记录尾节点
    	int length = 0;
    	
    	if(head==null){
    		return head;
    	}else{
    		length = 1;
    	}
    	
    	ListNode temp = head,tail = null;
    	while(temp.next!=null){
    		temp = temp.next;
    		length ++;
    	}
    	k = k%length;
    	if(length==1)
    		return head;
    	k = length - k - 1;
		tail = temp;
		
		//构成幻
		tail.next = head;
		
		
		//找到目标节点
		for(int i=0; i<k; i++){
			head = head.next;
		}
		
		temp = head.next;
		head.next = null;
		
		return temp;
		
	}
}
```
源代码，出门右转：[github/LeetCode](https://github.com/ldc4/LeetCode)