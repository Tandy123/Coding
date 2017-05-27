## 【牛客】剑指Offer面试题-高质量的代码

### 面试题16-数值的整数次方

#### 题目描述

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。
不得使用库函数，同时不需要考虑大数问题

#### 分析

要考虑全面（0次，负数次），高效（快速幂）

#### 代码

```c++
class Solution {
public:
    double Power(double base, int exponent) {
    	double res = 1;
        
        if(0.0 == base && exponent < 0){
            //inbalid input
            return 0.0;
        }
        res = PowerWithUnsignedExponent(base, abs(exponent));
         if(exponent < 0)
        {
            res = 1.0/res;
        }
        return res;
    }
    double PowerWithUnsignedExponent(double base, unsigned int exponent)
    {
        if(exponent == 0)
        {
            return 1;
        }
        if(exponent == 1)
        {
            return base;
        }
        
        double res = PowerWithUnsignedExponent(base, exponent >> 1);
        res *= res;
        if(exponent & 0x1 == 1)
        {
            res *= base;
        }
        return res;
    }
};
```