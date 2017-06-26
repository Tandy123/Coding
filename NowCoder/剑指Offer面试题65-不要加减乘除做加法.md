## 【牛客】剑指Offer面试题65

### 面试题65-不要加减乘除做加法

#### 题目描述
写一个函数，求两个整数之和，要求在函数体内不得使用+、-、\*、/四则运算符号。

#### 分析

用位运算，不进位相加可以用^来模拟实现，进位可以用&和<<来模拟实现

#### 代码
- 剑指Offer版
```c++
class Solution {
public:
    int Add(int num1, int num2)
    {
		int sum, carry;
        do{
            sum = num1 ^ num2;
            carry = (num1 & num2)<<1;
            num1 = sum;
            num2 = carry;
        }while(carry != 0);
        return sum;
    }
};
```
- 牛客大神简洁版
```c++
public class Solution {
    public int Add(int num1,int num2) {
        while (num2!=0) {
            int temp = num1^num2;
            num2 = (num1&num2)<<1;
            num1 = temp;
        }
        return num1;
    }
}
```
