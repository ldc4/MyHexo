categories:
  - Arithmetic
tags:
  - sort
  - arithmetic
title: 基本排序算法回顾
date: 2016-03-10 21:08:39
---

抱着初学者的心态又把排序算法过了一遍。
学习所谓已经成熟的算法，不是要去把它背下来，而是掌握其内在的思想，帮助你在解决问题的时候，思维的拓展。

<!--more-->

## 排序分类
内排序 - 待排序的所有记录全部被放置在内存中


外排序 - 由于排序的记录个数太多，不能同时放置在内存，需要在内外存之间多次交换数据


本质：比较和移动/交换


内排序分为：

     - 插入排序
          - 直接插入排序   略优于冒泡和简单选择
          - 希尔排序      希尔实现了o(nlogn)排序
     - 交换排序
          - 冒泡排序      三种实现
          - 快速排序      最强王者
     - 选择排序
          - 简单选择排序   略优于冒泡
          - 堆排序        利用了堆结构
     - 归并排序
          - 2路归并排序

---

## 排序实现

### 冒泡排序
``` java
//冒泡排序
public class BubbleSort {
	
	//两两比较相邻记录的关键字，如果反序则交换，直到没有反序记录为止
	
	//改进版冒泡排序:针对那种途中就排好序了，就不用再排了（可以避免因已经有序的情况下的无意义循环判断）
	//o(n^2)
	public void sort3(int[] arr){
		boolean flag = true;
		for(int i=0;i<arr.length-1&&flag;i++){
			flag = false;
			for(int j=arr.length-1;j>i;j--){
				if(arr[j-1]>arr[j]){
					int temp = arr[j-1];
					arr[j-1] = arr[j];
					arr[j] = temp;
					flag = true;
				}
			}
		}
		
	}
	
	
	//正宗冒泡排序，在不断循环中，除了把最小的关键字放到最上面，其他关键字也在往上走
	public void sort2(int[] arr){
		for(int i=0;i<arr.length-1;i++){
			for(int j=arr.length-1;j>i;j--){
				if(arr[j-1]>arr[j]){
					int temp = arr[j-1];
					arr[j-1] = arr[j];
					arr[j] = temp;
				}
			}
		}
	}
	
	//效率差，且不满足“相邻交换”
	public void sort1(int[] arr){
		for(int i=0;i<arr.length-1;i++){
			for(int j=i+1;j<arr.length;j++){
				if(arr[i]>arr[j]){
					int temp = arr[i];
					arr[i] = arr[j];
					arr[j] = temp;
				}
			}
		}
		
	}
}

```

### 简单选择排序

``` java
//简单选择排序
public class SimpleSelectSort {

	//通过n-i次关键字间的比较，从n-i+1个记录中选出关键字最小的记录，并和第i个记录交换。
	
	
	public void sort(int[] arr){
		int min;
		for(int i=0;i<arr.length-1;i++){
			min = i;
			for(int j=i+1;j<arr.length;j++){
				if(arr[min]>arr[j]){
					min = j;
				}
			}
			if(i!=min){
				int temp = arr[i];
				arr[i] = arr[min];
				arr[min] = temp;
			}
		}
	}
	
}
```

### 直接插入排序

``` java
//直接插入排序
public class StraightInsertionSort {

	//将一个记录插入到已经排好的有序表中，从而得到一个新的、记录增1的有序表
	
	public void sort2(int[] arr){
		for(int i=2;i<arr.length;i++){
			if(arr[i]<arr[i-1]){
				arr[0] = arr[i];//arr[0]是哨兵
				int j=i-1;
				for(;arr[j]>arr[0];j--){
					arr[j+1] = arr[j];
				}
				arr[j+1] = arr[0];
			}
		}
	}
	
	public void sort1(int[] arr){
		int temp;//使用临时变量来暂时存储待插入的值
		for(int i=1;i<arr.length;i++){
			if(arr[i]<arr[i-1]){
				temp = arr[i];
				int j=i-1;
				//在适用临时变量的时候需要注意j的值，它会减到负值，哨兵就不用考虑这个
				for(;j>=0&&arr[j]>temp;j--){
					arr[j+1] = arr[j];
				}
				arr[j+1] = temp;
			}
		}
	}
	
}
```

### 希尔排序

``` java
//希尔排序（最小增量排序）
public class ShellSort {
	
	//由于直接插入排序在记录数量少，基本有序的序列中效率高
	
	//希尔排序 就通过增量分组来创造这样的条件
	
	public void sort1(int[] arr){
		int increment = arr.length;//初始增量
		
		do{
			increment = increment/2;
			
			for(int i=increment+1;i<arr.length;i++){
				if(arr[i]<arr[i-increment]){
					arr[0] = arr[i];
					int j=i-increment;
					//这里需要加上j>0,是因为increment>1,最后判断条件的时候，下标会被减为负
					for(;j>0&&arr[j]>arr[0];j-=increment){
						arr[j+increment] = arr[j];
					}
					arr[j+increment] = arr[0];
				}
			}
		}while(increment>1);
		
	}
	
	public void sort2(int[] arr){
		int increment = arr.length;//初始增量
		int temp;
		do{
			//只要最后的增量变为1，你无论怎么设置增量都行
			increment = increment/5+1;
			
			for(int i=increment;i<arr.length;i++){
				if(arr[i]<arr[i-increment]){
					temp = arr[i];
					int j=i-increment;
					for(;j>=0&&arr[j]>temp;j-=increment){
						arr[j+increment] = arr[j];
					}
					arr[j+increment] = temp;
				}
			}
		}while(increment>1);
	}
}
```

### 堆排序

``` java
//堆排序
public class HeapSort {

	
	//利用堆进行排序
	
	public void sort(int[] arr){
		
		//将待排序列构建成一个大顶堆
		for(int i=arr.length/2-1;i>=0;i--){
			HeapAdjust(arr,i,arr.length);
		}
		
		//将每个最大值的根节点与末尾元素交换，再调整成大顶堆
		for(int i=arr.length-1;i>0;i--){
			
			int temp = arr[0];
			arr[0] = arr[i];
			arr[i] = temp;
			
			HeapAdjust(arr,0,i);
		}
		
	}

	//堆调整
	private void HeapAdjust(int[] arr,int rootIndex,int len) {
		
		int temp,j;
		temp = arr[rootIndex];
		for(j=2*rootIndex+1;j<len;j=2*j+1){
			if(j<(len-1)&&arr[j]<arr[j+1])
				++j;
			if(temp>=arr[j])
				break;
			arr[rootIndex] = arr[j];
			rootIndex=j;
		}
		arr[rootIndex]=temp;
	}
	
	//哨兵模式
	public void sort2(int[] arr){
		
		//将待排序列构建成一个大顶堆
		for(int i=arr.length/2;i>0;i--){
			HeapAdjust(arr,i,arr.length);
		}
		
		//将每个最大值的根节点与末尾元素交换，再调整成大顶堆
		for(int i=arr.length-1;i>1;i--){
			
			int temp = arr[1];
			arr[1] = arr[i];
			arr[i] = temp;
			
			HeapAdjust2(arr,1,i);
		}
		
	}
	private void HeapAdjust2(int[] arr,int rootIndex,int len) {
		
		int j;
		arr[0] = arr[rootIndex];
		for(j=2*rootIndex;j<len;j=2*j){
			if(j<(len-1)&&arr[j]<arr[j+1])
				++j;
			if(arr[0]>=arr[j])
				break;
			arr[rootIndex] = arr[j];
			rootIndex=j;
		}
		arr[rootIndex]=arr[0];
	}
}

```

### 归并排序

``` java
//归并排序
public class MergingSort {
	
	//归并就是把两个或两个以上有序数列合并成一个有序数列
	
	//非递归的迭代方法，避免了递归时深度为log2n的栈空间
	
	//通过迭代实现
	public void sort2(int[] arr){
		
		int[] tempArr = new int[arr.length];
		
		int step = 1;
		
		while(step<arr.length){
			MergePass(arr,tempArr,step);
			step=2*step;
			MergePass(tempArr,arr,step);
			step=2*step;
		}
		
	}
	
	//将arr1中相邻长度为s的子序列两两归并到arr2
	private void MergePass(int[] arr1, int[] arr2, int step) {
		
		int i=0;
		
		// * *  * *  * *  *
		// * * * *   * * * 
		while(i <= arr1.length-2*step){
			Merge(arr1, arr2, i, i+step-1, i+2*step-1);
			i = i+2*step;
		}
		
		if(i<arr1.length-step){
			//最后还剩一个单系列的时候，归并那个单序列
			Merge(arr1,arr2, i, i+step-1, arr1.length-1);
		}else{
			//直接复制过去
			for(int j=i;j<arr1.length;j++){
				arr2[j] = arr1[j];
			}
		}
		
	}


	//通过递归实现
	public void sort1(int[] arr){
		Msort(arr,arr,0,arr.length-1);
	}

	//将arr1[start...end]归并排序为arr2[start...end]
	private void Msort(int[] arr1, int[] arr2, int start, int end) {
		int mid;
		int[] tempArr = new int[arr2.length];
		
		if(start==end){
			arr2[start] = arr1[start];
		}else{
			// 0 1 2 3 [4] 5 6 7 8
			mid = (start+end)/2;
			Msort(arr1,tempArr,start,mid);
			Msort(arr1,tempArr,mid+1,end);
			Merge(tempArr,arr2,start,mid,end);
		}
		
	}
	//将有序的tempArr[start...mid]和有序的tempArr[mid+1...end]归并到arr
	//    [-----]  [-----]
	//     |   |    |   |
	//     s   m   m+1  e
	//     i        j
	//    mid不能变，要作为参考的标准，s可以变
	private void Merge(int[] tempArr, int[] arr, int start, int mid, int end) {
		int i=start,j=mid+1;
		//start记录arr下标
		//while更优雅
		while(i<=mid && j<=end){
			if(tempArr[i]<tempArr[j]){
				arr[start++] = tempArr[i++];
			}else{
				arr[start++] = tempArr[j++];
			}
		}
		while(i<=mid){
			arr[start++] = tempArr[i++];
		}
		while(j<=end){
			arr[start++] = tempArr[j++];
		}
		/*
		for(i=start,j=mid+1;i<=mid&&j<=end;start++){
			if(tempArr[i]<tempArr[j]){
				arr[start] = tempArr[i++];
			}else{
				arr[start] = tempArr[j++];
			}
		}
		if(i<=mid){
			for(int k=i;k<=mid;k++){
				arr[start++] = tempArr[k];
			}
		}
		if(j<=end){
			for(int k=j;k<=end;k++){
				arr[start++] = tempArr[k];
			}
		}
		*/
	}
	
}
```
### 快速排序

``` java
//快速排序
public class QuickSort {

	
	public void sort(int[] arr){
		QSort(arr,0,arr.length-1);
	}

	private void QSort(int[] arr, int low, int high) {
		
		int pivot;//枢纽
		
		
		if(low<high){
			pivot = Partition(arr,low,high);
			QSort(arr,low,pivot-1);
			QSort(arr,pivot+1,high);
		}/*
		while(low<high){
			pivot = Partition(arr,low,high);
			QSort(arr, low, pivot-1);
			low = pivot+1;
		}*/
		
	}

	private int Partition(int[] arr, int low, int high) {
		
		//pivotkey的选择是一个潜在的性能瓶颈
		int pivotkey = arr[low];
		//int temp = pivotkey;
		while(low<high){
			while(low<high&&arr[high]>=pivotkey){
				high--;
			}
			arr[low] = arr[high];
			/*
			int temp = arr[high];
			arr[high] = arr[low];
			arr[low] = temp;
			*/
			while(low<high&&arr[low]<=pivotkey){
				low++;
			}
			arr[high] = arr[low];
			/*
			temp = arr[high];
			arr[high] = arr[low];
			arr[low] = temp;
			*/
		}
		//arr[low] = temp;
		arr[low] = pivotkey;
		return low;
	}
}
```