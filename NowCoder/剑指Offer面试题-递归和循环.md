## 【牛客】剑指Offer面试题-递归和循环

### 面试题10-斐波那契数列

#### 题目描述

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。其中n<=39

#### 分析

三种方法实现：

- 递归：数据大的时候效率低，时间复杂度n的指数
- 循环：根据f(0)f(1)算出f(2)，再根据f(1)f(2)算出f(3)……，以此类推，时间复杂度O(n)
- 利用矩阵相乘：矩阵为{{1，1}，{1，0}}时间复杂度O(logn)

#### 代码
```c
class Solution {
public:
    int Fibonacci(int n) {
		if(n == 0)
            return 0;
        if(n == 1)
            return 1;
        int fibNMinusOne = 1;
        int fibNMinusTwo = 0;
        int fibN;
        for(int i = 2; i <= n; i++)
        {
            fibN = fibNMinusOne + fibNMinusTwo;
            fibNMinusTwo = fibNMinusOne;
            fibNMinusOne = fibN;
        }
        return fibN;
    }
};
```
### 面试题10-1-跳台阶

#### 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

#### 分析

注意初始的情况和斐波那契数列的区别

#### 代码

```c
class Solution {
public:
    int jumpFloor(int number) {
        int fib[3] = {0, 1, 2};
        if(number < 0)
            return -1;//error
        if(number < 3)
            return fib[number];
 		int fibNMinusOne = 2;
        int fibNMinusTwo = 1;
        int fibN = 3;
        for(int i = 3; i <= number; ++i)
        {
            fibN = fibNMinusOne + fibNMinusTwo;
            fibNMinusTwo = fibNMinusOne;
            fibNMinusOne = fibN;
        }
        return fibN;
    }
};
```
### 面试题10-2-变态跳台阶

#### 题目描述

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。n<=39

#### 分析

归纳证明得到 f(n)=2^(n-1)，证明过程如下：

f(1) = 1  
f(2) = f(2-1) + f(2-2)         //f(2-2) 表示2阶一次跳2阶的次数。   
f(3) = f(3-1) + f(3-2) + f(3-3)    
... 
f(n) = f(n-1) + f(n-2) + f(n-3) + ... + f(n-(n-1)) + f(n-n)    
  
说明：  
1）这里的f(n) 代表的是n个台阶有一次1,2,...n阶的 跳法数。   
2）n = 1时，只有1种跳法，f(1) = 1   
3) n = 2时，会有两个跳得方式，一次1阶或者2阶，这回归到了问题（1） ，f(2) = f(2-1) + f(2-2)    
4) n = 3时，会有三种跳得方式，1阶、2阶、3阶， 那么就是第一次跳出1阶后面剩下：f(3-1);第一次跳出2阶，剩下f(3-2)；第一次3阶，那么剩下f(3-3) 因此结论是f(3) = f(3-1)+f(3-2)+f(3-3)   
5) n = n时，会有n中跳的方式，1阶、2阶...n阶，得出结论：   
f(n) = f(n-1)+f(n-2)+...+f(n-(n-1)) + f(n-n) =>f(0) + f(1) + f(2) + f(3) + ... + f(n-1)   
6) 由以上已经是一种结论，但是为了简单，我们可以继续简化：   
    f(n-1) = f(0) + f(1)+f(2)+f(3) + ... + f((n-1)-1) = f(0) + f(1) + f(2) + f(3) + ... + f(n-2)   
    f(n) = f(0) + f(1) + f(2) + f(3) + ... + f(n-2) + f(n-1) = f(n-1) + f(n-1)   
    可以得出：   
    f(n) = 2*f(n-1) 

    

7) 得出最终结论,在n阶台阶，一次有1、2、...n阶的跳的方式时，总得跳法为： 

              | 1       ,(n=0 )  
f(n) =     | 1       ,(n=1 ) 

              | 2*f(n-1),(n>=2)

#### 代码

```c
class Solution {
public:
    int jumpFloorII(int number) {
		if(number <= 0)
            return 0;
        if(number == 1)
            return 1;
        int res = 1;
        for(int i = 2; i <= number; i++)
        {
            res *= 2;
        }
        return res;
    }
};
```

和横竖铺地砖本质上都是斐波那契数列
