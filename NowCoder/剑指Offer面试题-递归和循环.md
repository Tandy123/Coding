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
#### 补充

青蛙跳台阶和横竖铺地砖本质上都是斐波那契数列
