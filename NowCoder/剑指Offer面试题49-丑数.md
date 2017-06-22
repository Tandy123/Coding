## 【牛客】剑指Offer面试题

### 面试题49-丑数

#### 题目描述

把只包含因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

#### 分析

直接暴力找的话，会在非丑数上有许多不必要的运算  
这里书上提到了可以创建数组保存已经找到的丑数，用空间换时间：对于当前找到的所有丑数，假设下一个丑数是u，u必然等于前面某个丑数乘以2/3/5，假设这三个丑数分别为T2，T3，T5，我们从T2*2，T3*3，T5*5中挑出最小的一个数，作为接下里的丑数，然后对应的T值进行自增。总体思想就像是风水轮流转的感觉。

#### 代码
- 剑指Offer版：
```c
class Solution {
public:
    int GetUglyNumber_Solution(int index) {
        if(index <= 0){
            return 0;
        }
    	int* pUglyNumbers = new int[index];
        pUglyNumbers[0] = 1;
        int curIndex = 1;
        int *pMultiply2 = pUglyNumbers;
        int *pMultiply3 = pUglyNumbers;
        int *pMultiply5 = pUglyNumbers;
        while(curIndex < index){
            int min = Min(*pMultiply2 * 2, *pMultiply3 * 3, *pMultiply5 * 5);
            pUglyNumbers[curIndex] = min;
            while(*pMultiply2 * 2 <= min){
                ++pMultiply2;
            }
            while(*pMultiply3 * 3 <= min){
                ++pMultiply3;
            }
            while(*pMultiply5 * 5 <= min){
                ++pMultiply5;
            }
            ++curIndex;
        }
        int ugly = pUglyNumbers[index - 1];
        delete pUglyNumbers;
        return ugly;
    }
    int Min(int number1, int number2, int number3){
        int temp = min(number1, number2);
        return min(temp, number3);
    }
};
```
- 牛客大神版：
```c++
class Solution {
public://别人的代码就是精简，惭愧啊，继续学习。
    int GetUglyNumber_Solution(int index) {
        if (index < 7)return index;
        vector<int> res(index);
        res[0] = 1;
        int t2 = 0, t3 = 0, t5 = 0, i;
        for (i = 1; i < index; ++i)
        {
            res[i] = min(res[t2] * 2, min(res[t3] * 3, res[t5] * 5));
            if (res[i] == res[t2] * 2)t2++;
            if (res[i] == res[t3] * 3)t3++;
            if (res[i] == res[t5] * 5)t5++;
        }
        return res[index - 1];
    }
};
```