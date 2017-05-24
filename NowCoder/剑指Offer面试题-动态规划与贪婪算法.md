## 【牛客】剑指Offer面试题-动态规划与贪婪算法

### 动态规划的四个特点

1. 目标是求一个问题的最优解；
2. 整体问题的最优解是依赖各个子问题的最优解；
3. 把大问题分解成若干个小问题，这些小问题之间还有相互重叠的更小的问题；
4. 从上往下分析问题，从下往上求解问题。

### 面试题14-剪绳子

#### 题目描述

给你一根长度为n绳子，请把绳子剪成m段（m、n都是整数，n>1并且m≥1）。每段的绳子的长度记为k[0]、k[1]、……、k[m]。k[0] * k[1] * … * k[m]可能的最大乘积是多少？例如当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到最大的乘积18。


#### 分析

两种思路：

动态规划（时间O(n^2)，空间O(n)）：  
把从4到length的所有最优解都存在一个数组里，用子问题的最优解推出最终的最优解  

贪婪（时间O(1)，空间O(1)）：  
当n>=5时，每次尽可能多地剪长度为3的绳子，当剩下的绳子长度为4时，把绳子剪成长度为2的两段。  
数学证明如下：  
首先，当n>=5时，有2(n-2)>n和3(n-3)>n；  
另外，当n>=5时，3(n-2)>=2(n-2)；  
因此，我们要尽可能地多剪长度为3的绳子。  

#### 代码
```c
// ====================动态规划====================
int maxProductAfterCutting_solution1(int length)
{
    if(length < 2)
        return 0;
    if(length == 2)
        return 1;
    if(length == 3)
        return 2;

    int* products = new int[length + 1];
    products[0] = 0;
    products[1] = 1;
    products[2] = 2;
    products[3] = 3;

    int max = 0;
    for(int i = 4; i <= length; ++i)
    {
        max = 0;
        for(int j = 1; j <= i / 2; ++j)
        {
            int product = products[j] * products[i - j];
            if(max < product)
                max = product;

            products[i] = max;
        }
    }

    max = products[length];
    delete[] products;

    return max;
}

// ====================贪婪算法====================
int maxProductAfterCutting_solution2(int length)
{
    if(length < 2)
        return 0;
    if(length == 2)
        return 1;
    if(length == 3)
        return 2;

    // 尽可能多地减去长度为3的绳子段
    int timesOf3 = length / 3;

    // 当绳子最后剩下的长度为4的时候，不能再剪去长度为3的绳子段。
    // 此时更好的方法是把绳子剪成长度为2的两段，因为2*2 > 3*1。
    if(length - timesOf3 * 3 == 1)
        timesOf3 -= 1;

    int timesOf2 = (length - timesOf3 * 3) / 2;

    return (int) (pow(3, timesOf3)) * (int) (pow(2, timesOf2));
}
```
