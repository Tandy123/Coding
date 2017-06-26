## 【牛客】剑指Offer面试题63

### 面试题63-股票的最大利润

#### 题目描述
假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖交易该股票可能获得的利润是多少？例如一只股票在某些时间节点的价格为{9, 11, 8, 5, 7, 12, 16, 14}。如果我们能在价格为5的时候买入并在价格为16时卖出，则能收获最大的利润11。

#### 分析

动态规划，diff(i)表示卖出价为数组中第i个数字时可能获得的最大利润，这里要注意，题目要求必须买卖一次，且买卖不能在同一天，所以这里需要将前i-1天的最小值存下来

#### 代码
```c++
int MaxDiff(const int* numbers, unsigned length)
{
    if(numbers == nullptr && length < 2)
        return 0;
    int min = numbers[0];
    int maxDiff = numbers[1] - min;
    for(int i = 2; i < length; ++i)
    {
        if(numbers[i - 1] < min)
            min = numbers[i - 1];
        int currentDiff = numbers[i] - min;
        if(currentDiff > maxDiff)
            maxDiff = currentDiff;
    }
    return maxDiff;
}
```