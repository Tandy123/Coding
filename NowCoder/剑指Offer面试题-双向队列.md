## 【牛客】剑指Offer面试题-双向队列

### 面试题59-1-滑动窗口的最大值

#### 题目描述
给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

#### 分析
- 注意当窗口大于数组长度时，不存在窗口的最大值
- 解题关键：用一个双向队列，队列头是窗口最大值的索引号，遍历数组，当当前的数字大于队列头时，清空队列，压入当前值，当小于队列头时，从队列中清空比当前数字小的数，从尾部压入当前数字，总之就是用一个队列记录窗口的最大值的索引和可能成为窗口最大值的索引。

#### 代码
```c
class Solution {
public:
    vector<int> maxInWindows(const vector<int>& num, unsigned int size)
    {
        vector<int> maxInWindows;
        if(num.size() >= size && size >= 1){
            deque<int> index;
            for(int i = 0; i < size; ++i){
                while(!index.empty() && num[i] >= num[index.back()]){
                    index.pop_back();
                }
                index.push_back(i);
            }
            for(int i = size; i < num.size(); ++i){
                maxInWindows.push_back(num[index.front()]);
                while(!index.empty() && num[i]>num[index.back()]){
                    index.pop_back();
                }
                if(!index.empty() && i - index.front() >= size){
                    index.pop_front();
                }
                index.push_back(i);
            }
            maxInWindows.push_back(num[index.front()]);
        }
        return maxInWindows;
    }
};
```

### 面试题59-2-队列的最大值

#### 题目描述
请定义一个队列并实现函数max得到队列里的最大值，要求函数max、push_back和pop_front的时间复杂度都是O(1)

#### 分析
- 参考前一题的思路，可以把滑动窗口看成是一个队列，只是这里不需要加入窗口大小的限制
- 建议写成模板，提高代码的扩展性，为了保证每个数字的唯一性，用一个currentIndex来唯一表识当前的数字

#### 代码
```c
template<typename T> class QueueWithMax
{
public:
    QueueWithMax() : currentIndex(0)
    {
    }

    void push_back(T number)
    {
        while(!maximums.empty() && number >= maximums.back().number)
            maximums.pop_back();

        InternalData internalData = { number, currentIndex };
        data.push_back(internalData);
        maximums.push_back(internalData);

        ++currentIndex;
    }

    void pop_front()
    {
        if(maximums.empty())
            throw new exception("queue is empty");

        if(maximums.front().index == data.front().index)
            maximums.pop_front();

        data.pop_front();
    }

    T max() const
    {
        if(maximums.empty())
            throw new exception("queue is empty");

        return maximums.front().number;
    }

private:
    struct InternalData
    {
        T number;
        int index;
    };

    deque<InternalData> data;
    deque<InternalData> maximums;
    int currentIndex;
};
```