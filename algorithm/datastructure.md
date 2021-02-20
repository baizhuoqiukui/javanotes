# 数据结构

0（log2n）的解释：二分法N个数，不断除2直到等于1，那么二分的次数就是基本语句执行的次数，于是我们可以设次数为x，N*（1/2）^x=1；则x=logn,底数是2，

### 学会使用对数器

符号  > >代表/2  ，< <代表乘2，(n  < <2) || 1 = n*2+1

**^异或运算就是无进位相加**

0^N=N 

N^N=0

**满足交换律和结合律**

## 如何不申请额外空间交换a和b

int a = 甲 

int b = 乙

三行代码：

​	

```java
	a = a ^ b     // a = 甲 ^ 乙 b = 乙 
 
 	b = a ^ b  	  // a = 甲 ^ 乙 b = 乙 ^ 甲 ^ 乙  = 甲 ^ 乙 ^ 乙 = 甲 ^ 0 = 甲

	a = a ^ b	  // a = 甲 ^ 乙 ^ 甲 = 乙        b = 甲


```

**前提：a和b所指内存不能相同 要不会变成0** 

知道就完了慎用，面试问再说



## 一个数组中有一种数出现了奇数次，其他数都出现了偶数次，怎么找到并打印这种数

int eur=0;

​	eur=a ^ b....

将所有数异或，偶数相同的都为0，奇数异或为本身，返回这个数

## 

## 怎么把一个int类型的数提取他二进制最右侧的1出来，其他位置为0

 int t = n

 t =  n ^ (~ n + 1)



## 一个数组中有两种数出现了奇数次，其他数出现了偶数次，怎么找到并打印这两种数 

```java
public static void printOddTimesNum2(int[] arr) {
   int eor = 0;
   for (int i = 0; i < arr.length; i++) {
      eor ^= arr[i];
   }
   // a 和 b是两种数
   // eor != 0 == a ^ b
   // eor最右侧的1，提取出来，	这位a不等于b
   // eor :     00110010110111000
   // rightOne :00000000000001000
   int rightOne = eor & (-eor); // 提取出最右的1
   int rightOne = eor & (~eor+1); // 补码就是负数

   
   
   int onlyOne = 0; // eor'
   for (int i = 0 ; i < arr.length;i++) {
      //  arr[1] =  111100011110000
      // rightOne=  000000000010000
      if ((arr[i] & rightOne) != 0) {
         onlyOne ^= arr[i];//找到这位为1的所有数
      }
   }
   System.out.println(onlyOne + " " + (eor ^ onlyOne));
}
```

## 统计二进制1有几个

```java
public static int bit1counts(int N) {
   int count = 0;
   
   //   011011010000
   //   000000010000     1
   
   //   011011000000
   while(N != 0) {
      int rightOne = N & ((~N) + 1);
      count++;
      N ^= rightOne;
      // N -= rightOne
   } 
   return count;
   
}
```

## Master公式

**只针对递归问题子规模一致时使用**

T [n] = aT[n/b] + f (n)（直接记为T [n] = aT[n/b] + T (N^d)）

其中 a >= 1 and b > 1 是常量，其表示的意义是n表示问题的规模，a表示递归的次数也就是生成的子问题数，b表示每次递归是原来的1/b之一个规模，f（n）表示分解和合并所要花费的时间之和。

解法：
①当d<logb a时，时间复杂度为O(n^(logb a))
②当d=logb a时，时间复杂度为O((n^d)*logn)
③当d>logb a时，时间复杂度为O(n^d)