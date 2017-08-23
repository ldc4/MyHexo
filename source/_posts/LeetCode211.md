title: LeetCode No.211 Add and Search Word
date: 2016-04-12 16:26:50
categories:
- Arithmetic
tags:
- leetcode
- trie
---
Design a data structure that supports the following two operations:

	void addWord(word)
	bool search(word)

search(word) can search a literal word or a regular expression string containing only letters `a-z` or `.`. A `.` means it can represent any one letter.
For example:

	addWord("bad")
	addWord("dad")
	addWord("mad")
	search("pad") -> false
	search("bad") -> true
	search(".ad") -> true
	search("b..") -> true

Note:
You may assume that all words are consist of lowercase letters a-z.

<!--more-->

## 思路
这里有2个方法：
添加一个单词
查找一个单词，返回值是Boolean

添加的话，结构里面维护一个容器，
查找这里涉及到.的问题，

想法一：容器把字符串按照长度分类存储
查找的话就只需要半段字符串的长度，然后再去相应长度的字符串集合中查找

没测试，提交，WA

	Input:addWord("a"),search(".")
	Output:[false]
	Expected:[true]


好吧，添加的问题：
原：
```
    public void addWord(String word) {
        int len = word.length();
        ArrayList<String> list = container.get(len);
        if(list!=null){
            list.add(word);
            container.put(len, list);
        }
    }
```
改：
```
    public void addWord(String word) {
        int len = word.length();
        ArrayList<String> list = container.get(len);

        if(list==null)
            list = new ArrayList<String>();

        list.add(word);
        container.put(len, list);
    }
```

居然直接AC了，好开心！！！！


卧槽，时间还挺快的。21ms，最快的是19ms

想一下还能如何改进？
hint:You should be familiar with how a Trie works. If not, please work on this problem: Implement Trie (Prefix Tree) first.
看来是考察查找树，我直接用的HashMap和ArrayList

discuss:
TrieTree

## 我的AC代码
```
package net.ldc4.argorithm.leetcode.NO211;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

/**
 * 想法一：容器把字符串按照长度分类存储
	查找的话就只需要半段字符串的长度，然后再去相应长度的字符串集合中查找
 * @author ldc4
 */
public class WordDictionary {

	private Map<Integer,ArrayList<String>> container = null;
	
	public WordDictionary() {
		container  = new HashMap<Integer,ArrayList<String>>();
	}
	
    // Adds a word into the data structure.
    public void addWord(String word) {
        int len = word.length();
        ArrayList<String> list = container.get(len);
        
        if(list==null)
        	list = new ArrayList<String>();

    	list.add(word);
    	container.put(len, list);
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        int len = word.length();
        char[] wordChars = word.toCharArray();
        char[] dictWordChars = null;
        ArrayList<String> list = container.get(len);
    	if(list!=null){
    		Iterator<String> it = list.iterator();
    		String dictWord = null;
    		while(it.hasNext()){
    			dictWord = it.next();
    			dictWordChars = dictWord.toCharArray();
    			int i=0;
    			for(; i<len; i++){
    				if(wordChars[i]=='.')
    					continue;
    				else if(wordChars[i]!=dictWordChars[i]){
    					break;
    				}
    			}
    			if(i==len)
    				return true;
    		}
    	}
    	return false;
    }
}

// Your WordDictionary object will be instantiated and called as such:
// WordDictionary wordDictionary = new WordDictionary();
// wordDictionary.addWord("word");
// wordDictionary.search("pattern");
```
## 别人的AC代码
```
package net.ldc4.argorithm.leetcode.NO211;

import java.util.LinkedList;
import java.util.Queue;


/*
 Discuss上面的
 */
class TrieNode {
    // Initialize your data structure here.
    TrieNode[] children;
    boolean end;
    public TrieNode() {
        children = new TrieNode[26];
        end = false;
    }
}
public class WordDictionary2 {
    private TrieNode root;
    WordDictionary2() {
        root = new TrieNode();
    }

    // Adds a word into the data structure.
    public void addWord(String word) {
        TrieNode node = root;
        for(char c: word.toCharArray()) {
            if(node.children[c -'a'] == null) {
                node.children[c - 'a'] = new TrieNode();
            }
            node = node.children[c-'a'];
        }
        node.end = true;
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        Queue<TrieNode> queue = new LinkedList<TrieNode>();
        queue.offer(root);
        for(char c: word.toCharArray()) {
            int size = queue.size();
            if (c == '.') {
                for(int i = 0; i < size; i++) {
                    TrieNode node = queue.poll();
                    for(int j = 0; j <  26; j++){
                        if(node.children[j] != null) {
                            // System.out.println(j);
                            queue.offer(node.children[j]);
                        }
                    }
                }
            } else {
                for(int i = 0; i < size; i++) {
                    TrieNode node = queue.poll();
                    if(node.children[c -'a'] != null) {
                        queue.offer(node.children[c-'a']);
                    }
                }
            }        
        }
        while (!queue.isEmpty()) {
            // System.out.println(queue.peek().end);
            if (queue.poll().end == true) return true;
        }
        return false;        
    }
}
```

源代码，出门右转：[github/LeetCode](https://github.com/ldc4/LeetCode)