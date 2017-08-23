title: LeetCode No.187 Repeated DNA Sequences
date: 2016-04-24 21:35:07
categories:
- Arithmetic
tags:
- leetcode
- hash
- string
---

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.
Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.
For example,
Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",

Return:
["AAAAACCCCC", "CCCCCAAAAA"].


Subscribe to see which companies asked this question

<!--more-->

## 思路
题意：10个字符的子序列 有重复的就返回

思路：
1.首先不费脑力的肯定是暴力算法：（n-10）*n(kpm)
kpm具体怎么写的，不清楚了。
以及如何优化？能否发现其他规律？

ACGCAGGTTA CGTACGATCGATCATGCGGCTAG

去discuss吸收吸收思路

因为只有4个字符，所以分别用00 01 10 11 来区分他们。
由于固定10字节，所以10字符的字符串，只需要20bit来表示，且每种排序的结果都不同，一个int足够。
然后利用hash表来判断是否重复。


嗯，不错。虽然不是我想出来的

为了不额外判断，直接用字符的后3bit来区分。一共也就30位。


wa
More Details 
Input:"AAAAAAAAAAAA"
Output:["AAAAAAAAAA","AAAAAAAAAA"]
Expected:["AAAAAAAAAA"]


没有考虑重复的情况。。

把HashSet改成HashMap，value来作为标记状态

或者把字符串加入到Set里面，最后再用List包装一下。


Runtime: 71 ms

时间有点满。

看了看其他答案，恍然大悟，为什么要可以去映射为整数？

HashSet本身不就是干映射的事情的么..

hashcode

Runtime: 33 ms

提高了一半的时间


真不知道那一个7ms是如何写出来的，
一遇到这种什么字符串比较的，我就老想到KMP。。


## AC代码1
```
package net.ldc4.argorithm.leetcode.NO187;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

public class Solution {
	
    public List<String> findRepeatedDnaSequences(String s) {
    	
    	List<String> result = new ArrayList<String>();
        int len = s.length();
    	if(len < 10)
    		return result;
    	
    	HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
          
    	for(int i = 0; i < len-9; i++){
        	String ss = s.substring(i,i+10);
        	int val = 0;
        	for(char c : ss.toCharArray()){
        		val = val << 3;
        		val += c;
        	}
        	val = val << 2;
        	
        	if(map.get(val)==null){
        		map.put(val, 1);
        	}else if(map.get(val)==1){
        		map.put(val, 2);
        		result.add(ss);
        	}
        }
    	
    	return result;
    }
}
```
## AC代码2
```
package net.ldc4.argorithm.leetcode.NO187;

import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;

public class Solution2 {

    public List<String> findRepeatedDnaSequences(String s) {
    	
    	HashSet<String> result = new HashSet<String>();
        int len = s.length();
    	if(len < 10)
    		return new ArrayList<String>(result);
    	
    	HashSet<String> repeat = new HashSet<String>();
          
    	for(int i = 0; i < len-9; i++){
        	String ss = s.substring(i,i+10);
        	if(!repeat.add(ss)){
        		result.add(ss);
        	}
        }
    	
    	return new ArrayList<String>(result);
    }
}
```

源代码，出门右转：[github/LeetCode](https://github.com/ldc4/LeetCode)