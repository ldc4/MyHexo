title: LeetCode No.278 First Bad Version 
date: 2016-03-15 10:55:13
categories:
- Arithmetic
tags:
- leetcode
- binary search
---
You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which will return whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

开玩笑，我这种英语水平还需要翻译？
<!--more-->
## 心理过程

123456  假如 4 是坏的

为何感觉这题简单得有点诡异

双端搜索试试

```
for(int i=1,j=n;i<=j;i++,j--){
     if(isBadVersion(i))
          return i;
     if(!isBadVersion(j))
          return j-1;
}
```

华丽丽的给了个TLE

![](http://ldc4.qiniudn.com/images/LeetCode278/leetcode278-1.png)


设置一个步长向前找：

int k = 

在设置步长的时候突然想到还有一个东西叫二分查找
![](http://ldc4.qiniudn.com/images/LeetCode278/leetcode278-2.png)
```
int k = n/2;

if(isBad(k))
     就找左半部分（包括K）
else
     就找右边部分
```
```
    public int search(int start,int end){
        int k = (start + end)/2;

        if(start == end)
            return start;

        if(isBadVersion(k))
            search(start,k);
        else
            search(k+1,end);
        return 0;
    }
```

还是wa了2次  第一次没写return 0; 
![](http://ldc4.qiniudn.com/images/LeetCode278/leetcode278-3.png)


第二次WA 栈溢出；  递归的缘故



然后我又点了Discuss

可以这样改
```
while(start < end){
     int mid = (start + end)/2
     if(isBadVersion(mid))
          end = mid;
     else
          start = mid + 1;
}

 int mid = (start + end)/2
=> int mid = start + (end - start)/2;//这样做不会越界
```

ac  20ms


看似简单 却处处隐藏杀鸡！


Discuss有个16ms的  mid = start + ((end - start)>>1)

注意>> 比 +-优先级低

然而我AC这个才19Ms

不科学啊


if else换了个位置还提高1ms好开心

但是说好的16呢

## AC代码
``` java
public class Solution extends VersionControl{
	public int firstBadVersion(int n) {
		int start = 1,end = n;
		
		while(start < end){
		     int mid = start + ((end - start)>>1);
		     //System.out.println(mid);
		     if(isBadVersion(mid))
		          end = mid;
		     else
		          start = mid + 1;
		}
        return start;    
    }
}
```

源代码，出门右转：[github/LeetCode](https://github.com/ldc4/LeetCode)
