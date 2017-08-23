title: 笔试题总结
date: 2016-04-04 16:35:31
categories:
- ForOffer
tags:
- writter-test
---
笔试了3场了，都很炸，但是起码会做一部分。
有些事情，身不由己，你必须得这样做，物竞人择，适者生存。
解决办法：努力去补笔试题，面试经历。另外，语言表达沟通交流能力很重要！
阿里的笔试在中下旬，还有近半个月的时间。趁这段时间，好好补一下。

已笔试：网易、360、腾讯

2016年4月9日10:28:18
更新，京东笔试
<!--more-->
选择题，多是java基础，计算机基础，夹杂着一些智力题。
不做总结，题目太多，记不下来=-=

编程题，咱们挨个来A一遍。线下有时间，任性~！
# 网易笔试题

## 简答题
### 异常框架
Throwable - Error
       |
Exception - RuntimeException
       |
(CheckedException)

常见CheckedException: ClassNotFoundException、IOException、SQLException
常见RuntimeException: ArithmeticException、ClassCastException、ConcurrentModificationException、IllegalArgumentException、NullPointerException

### wait()、notify()、notifyAll()需要在同步方法/代码块中使用么？为什么？
必须在同步方法、或者同步块中使用，否则会抛出IllegalMonitorStateException.
这是因为，如果没有锁，wait和notify有可能会产生竞态条件
如果在wait之前执行了notifyAll,那对象将会一直等待。
JVM通过在执行的时候抛出IllegalMonitorStateException的异常，来确保wait, notify时，获得了对象的锁，从而消除隐藏的Race Condition。
如果一个方法不需要相互排斥，不需要监测或线程之间协作，每一个线程可以自由访问此方法，那就不需要协作。

### volatile关键字的作用
volatile用来修饰变量，使一个变量的读写变为原子性操作


## 编程题
### 给定一个数（N>3），给出所有的和为N的连续递增序列
```
package net.ldc4.argorithm.foroffer;

/**
 * 2016年网易实习笔试题第一题
 * @author ldc4
 */
public class Netease_1 {
	//给定一个数（N>3），给出所有的和为N的连续递增序列
	
	//思路1：从1->N/2遍历，如i=2时，2,3,...,X逐项递加，加到和为N为止。
	//思路2：利用N项和公式，a,...,b，b^2+b = a^2-a + 2*N，这样只需要遍历a了
	//对于输入MAX_VALUE，其精度有影响
	public static void main(String[] args) {
		long t1 = System.currentTimeMillis();
		solution2((Integer.MAX_VALUE-10));
		long t2 = System.currentTimeMillis();
		System.out.println(t2-t1);
	}
	
	private static void solution2(int n){
		long N = n;
		long x; //中间结果
		double result; //求得的b,下面循环的i表示a
		for(long i=1; i<=N/2; i++){
			x = i*(i-1) + 2*N;
			result = Math.sqrt(x + 0.25) - 0.5;
			//判断result是否是整数
			if(result == (long)result){
				System.out.print(i+":");
				long j;
				for(j=i;j<result;j++){
					System.out.print(j+":");
				}
				System.out.println(j+"=");
			}
		}
	}
	
}
```
### 给定2个字符串，匹配最长子字符串
```
package net.ldc4.argorithm.foroffer;

/**
 * 2016年网易实习笔试题第二题
 * @author ldc4
 */
public class Netease_2 {
	//给定2个字符串，匹配最长子字符串
	
	public static void main(String[] args) {
		System.out.println(solution2("ahui", "shaohui"));
	}
	
	
	/*
	 把字符串看成两把直尺，相互往中间移动
	 */
	private static int solution2(String X,String Y){
		int xlen = X.length();
		int ylen = Y.length();
		char[] x = X.toCharArray();
		char[] y = Y.toCharArray();
		int max = -1 , curMax = max ,len = xlen+ylen;
		int mi=0,mj=0;
		int m=0,n=0;//x字符串起始位置为m，y字符串起始位置为n
		
		for(int i=0;i<len;i++){
			if(i<ylen)
				n = ylen - i;
			else
				m = i - ylen;
			
			curMax = 0;
			int j = 0;
			for(;m+j<xlen&&n+j<ylen;j++){
				if(x[m+j]==y[n+j])
					curMax++;
				else{
					if(curMax>max){
						max = curMax;
						mi = m+j-1;
						mj = n+j-1;
					}
					curMax = 0;
				}
			}
			//这里是全匹配的情况下需要保存curMax的
			if(curMax>max){
				max = curMax;
				mi = m+j-1;
				mj = n+j-1;
			}
			
		}
		//打印字符串
		char[] sub = new char[max];
		for(int i=mi,j=mj,k=max-1;i>=0&&j>=0;i--,j--){
			if(x[i]!=y[j])
				break;
			sub[k--] = x[i];
		}
		System.out.println(new String(sub));
		
		return max;
	}
	
	//思路：dp
	/*
	用c[i][j]表示Xi和Yj的最大SubString长度
	 如果 xi == yi, 则 c[i][j] = c[i-1][j-1]+1
	 如果 xi != yi, 则 c[i][j] = 0;
	 */
	private static int solution1(String X,String Y){
		int xlen = X.length();
		int ylen = Y.length();
		char[] x = X.toCharArray();
		char[] y = Y.toCharArray();
		
		int[][] c = new int[xlen+1][ylen+1];
		
		int max = -1,mi = 0,mj = 0;
		
		for(int i=1;i<=xlen;i++){
			for(int j=1;j<=ylen;j++){
				
				if(x[i-1] == y[j-1]){
					c[i][j] = c[i-1][j-1] + 1;
				}else{
					c[i][j] = 0;
				}
				
				if(c[i][j]>max){
					max = c[i][j];
					mi = i;
					mj = j;
				}
			}
		}
		
		//打印字符串
		char[] sub = new char[max];
		for(int i=mi-1,j=mj-1,k=max-1;i>=0&&j>=0;i--,j--){
			if(x[i]!=y[j])
				break;
			sub[k--] = x[i];
		}
		System.out.println(new String(sub));
		
		return max;
	}
}
```

# 腾讯模拟考试题

## 格雷码数
```
package net.ldc4.argorithm.foroffer;

/**
 * 腾讯模拟笔试第一题
 * @author ldc4
 */

public class TX_MN_GrayCode {
	//当我做到现在才知道它说的n位格雷码是个什么意思了。。。
	public static void main(String[] args) {
		for(String str : getGray2(4)){
			System.out.println(str);
		}
	}
	
	public static String[] getGray(int n) {
		int t = n;
		int m = 1;
        while(t!=0){
            t/=2;
            m*=2;
        }
        String[] temp = gGray(m/2);
        String[] result = new String[n+1];
        for(int i=0;i<=n;i++){
            result[i] = temp[i];
        }
        return result;
        
    }
    private static String[] gGray(int n){
        
		if(n==1)
            return new String[]{"0","1"};
        String[] grays = gGray(n/2);
        String[] temp = new String[2*n];

        int k = 0;
        //正序，加前缀 0
        for(int i=0; i<n; i++,k++){
            temp[k] = "0" + grays[i];
        }
        //倒序，加前缀 1
        for(int j=n-1 ;j>=0;j--,k++){
            temp[k] = "1" + grays[j];
        }
        
        return temp;
        
    }
    
    //n位格雷码
    public static String[] getGray2(int n) {
        if(n==1)
            return new String[]{"0","1"};
        String[] grays = getGray2(n-1);
        int m = (int)Math.pow(2,n);
        String[] temp = new String[m];

        int k = 0;
        //正序，加前缀 0
        for(int i=0; i<m/2; i++,k++){
            temp[k] = "0" + grays[i];
        }
        //倒序，加前缀 1
        for(int j=m/2-1 ;j>=0;j--,k++){
            temp[k] = "1" + grays[j];
        }
        
        return temp;
    }
    
}
```
## 找出超过一半数量的同金额红包
```
package net.ldc4.argorithm.foroffer;

import java.util.*;


/**
 * 腾讯模拟笔试第二题
 * @author ldc4
 */
public class TX_MN_Gift {
	//找出半数以上相同金额的红包
	public static void main(String[] args) {
		int[] gifts = new int[]{1,2,3,5,2,5,6,3,2,2,2,4,5,2,5,2,2,2,2,2,2,2,2,2};
		int result = getValue(gifts, gifts.length);
		System.out.println(result);
	}
	
    public static int getValue(int[] gifts, int n) {
    	Arrays.sort(gifts);
        int result = gifts[n/2];
        //如果没有一半以上的红包，返回0
        if(!isSelf(gifts,result)){
        	return 0;
        }
        
    	return result;
    }

	private static boolean isSelf(int[] gifts,int x) {
		int i=0;
		for(int gift:gifts){
			if(gift == x){
				i++;
			}
		}
		
		if(i>gifts.length/2)
			return true;
		else
			return false;
	}
}
```
## 大整数相乘
```
package net.ldc4.argorithm.foroffer;

import java.math.BigInteger;

/**
 * 腾讯模拟笔试第三题
 * @author ldc4
 */
public class TX_MN_BigIntMulti {
	//大整数相乘
	public static void main(String[] args) {
		BigInteger a = new BigInteger("1234");
		BigInteger b = new BigInteger("1111");
		System.out.println(Multi(a, b));
		
		//---没有处理前导0
		int[] x = {1,2,3,4};
		int[] y = {1,1,1,1};
		int[] z = MyMulti(x, y);
		for(int i:z){
			System.out.print(i);
		}
		System.out.println();
	}
	
	public static BigInteger Multi(BigInteger a,BigInteger b){
		return a.multiply(b);
	}
	
	public static int[] MyMulti(int[] x,int[] y){
		int xlen = x.length;
		int ylen = y.length;
		
		int xstart = xlen - 1;
		int ystart = ylen - 1;
		
		int[] z = new int[xlen+ylen];
		
		int carry = 0;//进位
		
		for(int i=xstart; i>=0; i--){
			int j,k;
			carry = 0;
			for(j=ystart,k=ystart+i+1; j>=0; j--,k--){
				z[k] = x[i]*y[j]+z[k]+carry;
				carry = z[k]/10;
				z[k] = z[k]%10;
			}
			z[k] = carry;
		}
		
		return z;
	}
}

```
# 360笔试题

## 第一题
给定n,小A选a,小B（女）选b,随机一个数x, 谁离x近谁赢，一样的距离，小B赢，1<=a,b,x<=n

```
package net.ldc4.argorithm.foroffer;
import java.util.*;

/**
 * 360笔试题第一题
 * @author ldc4
 */
public class _360_1{
	//给定n,小A选a,小B（女）选b,随机一个数x, 谁离x近谁赢，一样的距离，小B赢，1<=a,b,x<=n
	
	//其实结果，就是在B的左右，如果B在左半段，A就在右边，B在右半段，A就在左边。当B在中间的时候，左右都可以
	//下面是用的暴力算法
	public static void main(String[] args){
    
          Scanner scan = new Scanner(System.in);
          while(scan.hasNext()){
          
                int n = scan.nextInt();
                int b = scan.nextInt();
                
                //用来计数
                int[] flag = new int[n];
                
                int a = 1;
                for(int i=1; i<=n; i++){//i代表小A选的数
                      if(i==b)
                            continue;
                      for(int j=1; j<=n; j++){//j代表roll到的数
                      
                            if(foo(j-b)>foo(j-i))
                                  flag[i-1]++;
                      }
                      
                }
                
                int max = 0;
                for(int i=0;i<n;i++){
                	if(flag[i]>max){
                          max = flag[i];
                          a = i+1;
                	}
                	if(flag[i]==max){
                		
                	}
                }
                System.out.println(a);
                
          }
    }
                
    private static int foo(int a){
    	if(a<0)
              return -a;
    	return a;
    }
}

```


## 第二题
长度n的字符串,例如“.h..hz....”n=10,设f(str)为用“..”替换成“.”，一次替换一个，直到没有“..”为止的次数。
通过指定位置的字符替换后，计算f(str)的值。
```
package net.ldc4.argorithm.foroffer;

import java.util.Scanner;

/**
 * 360笔试题第二题
 * @author ldc4
 */
public class _360_2 {

	//长度n的字符串,例如“.h..hz....”n=10,设f(str)为用“..”替换成“.”，一次替换一个，直到没有“..”为止的次数。
	//通过指定位置的字符替换后，计算f(str)的值。

	public static void main(String[] args) {

		Scanner scan = new Scanner(System.in);
		while (scan.hasNext()) {
			int n = scan.nextInt();
			int m = scan.nextInt();

			String str = scan.next();
			for (int i = 0; i < m; i++) {
				int x = scan.nextInt();
				String cs = scan.next();
				char c = cs.toCharArray()[0];
				char[] ss = str.toCharArray();
				ss[x] = c;
				String result = new String(ss);
				
				System.out.println(foo(result));
			}

		}
	}

	private static int foo(String str) {
		
		int level = 0;
		while(str.contains("..")){
			str = str.replaceFirst("\\.\\.", ".");
			level++;
		}
		
		return level;
	}
}
```
# 腾讯笔试题
## stack 与 heap的区别
stack：栈，后进先出的线性结构
heap：堆，一种特殊的树形结构

JVM中，运行时数据区域包含有java堆，虚拟机栈与本地方法栈。

由于Stack的内存管理是顺序分配的，而且定长，不存在内存回收问题；
而Heap 则是随机分配内存，不定长度，存在内存分配和回收的问题；

## 给一个字符串，删除其中某些字符达到最长回文字符串，返回长度
```
package net.ldc4.argorithm.foroffer;


/**
 * 2016年腾讯笔试题第一题
 * 参考网上的Longest Palindromic Subsequence
 * @author ldc4
 */
public class TX_1 {
	//给一个字符串，删除其中某些字符达到最长回文字符串，返回长度
	
	/*
	 思路：求最长回文子串有4种方法：暴力，DP，中心扩展，Manacher算法
	 但是，这题是删除某些字符。
	 删除字符的策略？
	 本身半段回文字符串就要n^2算法时间复杂度，再去针对每个删除情况来判断，是行不通的
	 这个问题其实就是Longest Palindromic Subsequence
	 
	 最优子结构：
	 设L[0,n-1]为X[0,n-1]的最长回文子序列。
	 1.如果X[0] == X[n-1],则L[0,n-1] = L[1,n-2] + 2
	 2.如果不等，则L[0,n-1] = max{L[1,n-1],L[0,n-2]}
	 */
	public static void main(String[] args) {
		String str = "acmerandacm";
		char[] seq = str.toCharArray();
		int result = lps(seq, 0, seq.length - 1);
		int result2 = lpsDP(seq);
		System.out.println(result+":"+result2);
	}
	
	
	private static int lpsDP(char[] seq){
		int len = seq.length;
		int[][] dp = new int[len][len];
		int temp,a,b;
		
		for(int i=0;i<len;i++){
			dp[i][i] = 1;
		}
		
		for(int i=1;i<len;i++){
			temp = 0;
			
			for(int j=0;j+i<len;j++){
				
				if(seq[j] == seq[j+i])
					temp = dp[j+1][j+i-1] + 2;
				else{
					a = dp[j+1][j+i];
					b = dp[j][j+i-1];
					temp = a>b?a:b;
				}
				
				dp[j][j+i] = temp;
			}	
		}
		return dp[0][len-1];
	}
	
	private static int lps(char[] seq, int i,int j){
		if(i==j)
			return 1;
		
		if(i>j)
			return 0;
		
		if(seq[i] == seq[j]){
			return lps(seq,i+1,j-1)+2;
		}
		
		int a = lps(seq,i,j-1);
		int b = lps(seq,i+1,j);
		
		return a>b?a:b;
	}
}
```
## 蛇型矩阵
     1 2 3
     8 9 4
     7 6 5
     输出N*N的蛇形矩阵


```
package net.ldc4.argorithm.foroffer;

/**
 * 2016年腾讯第二道编程题
 * 在网上找的2个方法
 * @author ldc4
 */
public class TX_2 {
	//蛇形矩阵
	static int length = 7;
	static int value = 1;
	static int[][] snake = new int[length][length];
	static Direction lastDirection = Direction.Right;

	static enum Direction {
		Right, Down, Left, Up;
	}

	public static void initialArray() {
		int row = 0, line = 0;
		for (int c = 0; c < length * length; c++) {
			snake[row][line] = value;
			lastDirection = findDirection(row, line);
			switch (lastDirection) {
			case Right:
				line++;
				break;
			case Down:
				row++;
				break;
			case Left:
				line--;
				break;
			case Up:
				row--;
				break;
			default:
				System.out.println("error");
			}
			value++;
		}
	}

	static Direction findDirection(int row, int line) {
		Direction direction = lastDirection;
		switch (direction) {
		case Right: {
			if ((line == length - 1) || (snake[row][line + 1] != 0))
				direction = direction.Down;
			break;
		}
		case Down: {
			if ((row == length - 1) || (snake[row + 1][line] != 0))
				direction = direction.Left;
			break;
		}
		case Left: {
			if ((line == 0) || (snake[row][line - 1] != 0))
				direction = direction.Up;
			break;
		}
		case Up: {
			if (snake[row - 1][line] != 0)
				direction = direction.Right;
			break;
		}
		}
		return direction;
	}

	public static void main(String[] args) {
		initialArray();

		// display.....
		for (int i = 0; i < length; i++) {
			for (int j = 0; j < length; j++) {
				System.out.print(snake[i][j] + "  ");
			}
			System.out.println();
		}
	}
	
	
/*    
    public static void main(String[] args){
        int i=11;
        TX_2.print(TX_2.fill(i));        
    }

     * 
     * 算法复杂度为n
     * 以下的算法，在for循环内的一些计算是不必要的，可以用变量保存，但是为了代码更加直观，就不做优化了。
     * 
     * @param i 矩阵边长
     *
    public static int[][] fill(int i){
        Long startTime=System.currentTimeMillis();
        //第几圈
        int count=0;
        //数列长度
        int step;
        //总圈数
        int all;
        //某圈开始计数的基数
        int startNum=0;
        //用于数组下标计算
        int startIndex;
        int k;
        int[][] array=null;
        if(i>0){
            array=new int[i][i];
            all=i/2+i%2;
            while(all>=count){
                step=i-1-(count<<1);
                count++;
                                
                for(startIndex=count-1,k=1;k<step+1;k++){
                    array[count-1][startIndex++]=startNum+k;    
                }
                
                for(startIndex=count-1,k=1;k<step+1;k++){
                    array[startIndex++][i-count]=startNum+k+step;
                }
                
                for(startIndex=i-count,k=1;k<step+1;k++){
                    array[i-count][startIndex--]=startNum+k+2*step;
                }
                
                for(startIndex=i-count,k=1;k<step+1;k++){
                    array[startIndex--][count-1]=startNum+k+3*step;
                }
                startNum=4*step+startNum;
            }
            //当矩阵边长为基数时，打印最后一个数字
            if(i%2>0){
                int mid=all-1;
                array[mid][mid]=i*i;
            }
        }
        Long timeUsed=System.currentTimeMillis()-startTime;
        System.out.println("总用时："+timeUsed+"ms");
        return array;        
    }
    public static void print(int[][] array){
        if(array!=null){
            int n=array.length;
            int i=0,j;
            int count=Integer.valueOf(n*n).toString().length()+1;
            for(;i<n;i++){
                for(j=0;j<n;j++){
                    System.out.printf("%"+count+"d",array[i][j]);
                }
                System.out.println();
            }
        }
    }
 */
}
```
## 写一个sql
     
FUid    FName  FScore
123     奔驰       80
465     宝马       55
123     宝马       70
789     奥迪       50
465     奥迪       100

查询作过2个或2个以上汽车评分的用户信息，并按照评分品均值排名。

容我去看看w3cshool sql再来尝试写写。

我灰来了：其实sql满简单的。

![](http://ldc4.qiniudn.com/images/SummaryOfWrittenTest/SummaryOfWrittenTest-1.png)

# 京东笔试题
25道选择题，百度了几道，手机百度真是不好用。
选择题一直是做得轻飘飘的。

2道编程题只做了一道，然后被绕在了逻辑里面了，提交试卷后，改好了，但是不知道能AC不。

第一题有点看不懂，这里我就假设它的最短路径是先通过斜角到达T的垂直方向。

京东和360都是用的赛码网，虽然copy了原题下来，但是篇幅太长了，毕竟能看到这篇文章的，有心看下去的都是经历过这些笔试的。
说出题目大概就能知道原题的。

## 路径规划
给一个棋盘，（a-h,1-8）为棋盘的坐标。给定S,T 2个位置，然后输出最短最优路径，步长和过程，优先走对角。
```
package net.ldc4.argorithm.foroffer;

import java.util.Scanner;

/**
 * 思路：
 * 第一题有点看不懂，这里我就假设它的最短路径是先通过斜角到达T的垂直方向。
 * @author ldc4
 */
public class JD_1 {

	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		while(scan.hasNext()){
			String S = scan.next();
			String T = scan.next();
			
			if(S.equals(T))
				continue;
			
			char[] s = S.toCharArray();
			char[] t = T.toCharArray();
			
			//两个点的位置情况一共有12种情况
			//先不去管位置情况，我只需要得到坐标差值,正数代表向右或向上，负数代表向左或向下
			
			int x = t[0] - s[0];
			int y = t[1] - s[1];
			
			int absx = Math.abs(x);
			int absy = Math.abs(y);
			int step = absx > absy? absx:absy;
			System.out.println(step);
			while(x!=0 || y!=0){
				if(x==0 && y>0){
					System.out.println("U");
					y--;
				}else
				if(x>0 && y==0){
					System.out.println("R");
					x--;
				}else
				if(x==0 && y<0){
					System.out.println("D");
					y++;
				}else
				if(x<0 && y==0){
					System.out.println("L");
					x++;
				}else
				if(x>0 && y>0){
					System.out.println("RU");
					x--; y--;
				}else
				if(x>0 && y<0){
					System.out.println("RD");
					x--; y++;
				}else
				if(x<0 && y>0){
					System.out.println("LU");
					x++; y--;
				}else
				if(x<0 && y<0){
					System.out.println("LD");
					x++; y++;
				}
			}
			
			
			
		}
	}
}
/*
a8
h1

7
RD
RD
RD
RD
RD
RD
RD
---
a8
h4

7
RD
RD
RD
RD
R
R
R

*/
```

## 三子棋
给一个棋盘，让你判定状态以下：
	1：轮到先手下棋；
	2：轮到后手下棋；
	x：给定的棋局不是合法的棋局；
	1 won：先手获胜；
	2 won：后手获胜；
	Draw：平局；
```
package net.ldc4.argorithm.foroffer;

import java.util.Scanner;

/**
 * 思路：
 * 按照流程来。
 * 分3类：
 * 1.非合法棋局：    x
 * 2.含有.的合法棋局：   1 ，2
 * 3.不含有.的合法棋局： 1 won,2 won, draw
 * @author ldc4
 */
public class JD_2 {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		while(scan.hasNext()){
			String a1 = scan.next();
			String a2 = scan.next();
			String a3 = scan.next();
			
			char[][] map = new char[3][3];
			//得到棋局
			for(int i=0; i<3; i++){
				map[0][i] = a1.charAt(i);
				map[1][i] = a2.charAt(i);
				map[2][i] = a3.charAt(i);
			}
			//判断是否是合法棋局（画X的人先开始，即X=O或者X=O+1）
			int xlen = 0,olen = 0,dlen = 0;
			
			for(int i=0; i<3; i++){
				for(int j=0; j<3; j++){
					if(map[i][j] == 'X')
						xlen++;
					else if(map[i][j] == '0'){
						olen++;
					}else{
						dlen++;
					}
				}
			}
//			System.out.println("xlen="+xlen);
//			System.out.println("olen="+olen);
//			System.out.println("dlen="+dlen);
			int xflag = win(map,'X');
			int oflag = win(map,'0');
			//合法条件：没有结束，结束x赢，结束o赢
			if((xflag==0&&oflag==0&&(xlen==olen||xlen==olen+1))||(xflag==1&&oflag==0&&xlen==olen+1)||(xflag==0&&oflag==1&&xlen==olen)){
			
				//如果不是完局
				if(dlen!=0&&xlen==olen&&oflag!=1){
					System.out.println("1");
				}else if(dlen!=0&&xlen==olen&&oflag==1){
					System.out.println("2 won");
				}
				
				if(dlen!=0&&xlen==olen+1&&xflag!=1){
					System.out.println("2");
				}else if(dlen!=0&&xlen==olen+1&&xflag==1){
					System.out.println("1 won");
				}

				//还剩 下完棋局 的情况，判断谁赢
				if(dlen==0){
					if(xflag==1&&oflag==0){
						System.out.println("1 won");
					}else if(xflag==0&&oflag==1){
						System.out.println("2 won");
					}else if(xflag==0&&oflag==0){
						System.out.println("draw");
					}
				}
			}else{
				System.out.println("x");
			}
			
			
		}
	}
	
	private static int win(char[][] map,char x){
		int flag = 0;
		if(map[0][0]==x&&map[0][1]==x&&map[0][2]==x)
			flag++;
		if(map[0][0]==x&&map[1][1]==x&&map[2][2]==x)
			flag++;
		if(map[0][0]==x&&map[1][0]==x&&map[2][0]==x)
			flag++;
		if(map[0][2]==x&&map[1][1]==x&&map[2][0]==x)
			flag++;
		if(map[2][0]==x&&map[2][1]==x&&map[2][2]==x)
			flag++;
		if(map[2][2]==x&&map[1][2]==x&&map[0][2]==x)
			flag++;
		if(map[0][1]==x&&map[1][1]==x&&map[2][1]==x)
			flag++;
		if(map[1][0]==x&&map[1][1]==x&&map[1][2]==x)
			flag++;
		return flag;
	}
}

/*
case:

X00
X0.
X..

...
...
...

X0X
X00
0X.

.X.
...
...

..X
...
...

0..
...
...

X0X
.0.
.X.

.X.
0X.
0X.

0X0
X0X
XX0


 */

```




代码位置:[LeetCode->net.ldc4.argorithm.foroffer](https://github.com/ldc4/LeetCode/tree/master/src/net/ldc4/argorithm/foroffer)