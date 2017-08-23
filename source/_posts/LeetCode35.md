title: LeetCode No.35 Search Insert Position
date: 2016-03-29 14:09:17
categories:
- Arithmetic
tags:
- leetcode
- binary search
---
Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
You may assume no duplicates in the array.
Here are few examples.
[1,3,5,6], 5 → 2
[1,3,5,6], 2 → 1
[1,3,5,6], 7 → 4
[1,3,5,6], 0 → 0

Subscribe to see which companies asked this question

<!--more-->

## 思路
>
前提：有序的数组

1.查找，找到
     二分查找
     
2.没找到，返回插入后的索引
     [1,3,5,7,9,10] 
->6 <7
     [1,3,5,7]
->6>5
     [5,7]
->6>5
index(6) = index(5)+1
>


mid = (end + start)/2  而不是 (end - start)/2
考虑边界问题 start < = end ，即当数组为单元素。
事实上start=end时如果target<nums[mid] end = mid = start,就会一直做空循环

        if(end == 0 && target<=nums[end])
            return 0;
        if(end == 0 && target>nums[end])
            return 1;

但是感觉这样是不是有点麻烦啊，有简便的方法嘛？

居然wa了。
Input:[1,3] 4
Output:1
Expected:2


....醉了。


做特殊情况处理


        //边界
        if(end == 0 && target<=nums[end])
            return 0;

        //特殊情况
        if(target>nums[end])
            return end + 1;


AC


有了特殊情况，边界 只有一个元素的判断就没用了

## AC代码

```
public class Solution {
    public int searchInsert(int[] nums, int target) {
        
        
        int start = 0;
        int end = nums.length-1;
        int mid;
        
        //特殊情况
        if(target>nums[end])
        	return end + 1;
        
        while(start<end){
        	mid = (end + start)/2;
        	
            if(target == nums[mid])
                return mid;
                
            if(target<nums[mid])
                end = mid;
            else
                start = mid + 1;
        }
        
        return start;
    }
    
}
```
源代码，出门右转：[github/LeetCode](https://github.com/ldc4/LeetCode)