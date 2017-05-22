## 【牛客】剑指Offer面试题-数组

### 面试题4-二维数组中的查找

#### 题目描述

在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

#### 分析

从右上或者左下开始查找

#### 代码

```c
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        bool found = false;
        if(array.size() > 0 && array[0].size() > 0)
        {
            int row = 0;
            int column = array[0].size()-1;
            while(row < array.size() && column >= 0)
            {
                if(array[row][column] == target)
                {
                    found = true;
                    break;
                }
                else if(array[row][column] > target)
                {
                    --column;
                }else
                {
                    ++row;
                }
            }
        }
        return found;
    }
};
```