## 【牛客】剑指Offer面试题53

### 面试题53-1-数字在排序数组中出现的次数

#### 题目描述

统计一个数字在排序数组中出现的次数。

#### 分析

递归实现二分查找

#### 代码

```c++
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        int number = 0;
        if(data.size() > 0){
            int first = GetFirstK(data, k, 0, data.size()-1);
            int last = GetLastK(data, k, 0, data.size()-1);
            if(first > -1 && last >-1){
                number = last - first + 1;
            }
        }
        return number;
    }
    int GetFirstK(const vector<int> &data, int k, int start, int end){
        if(start > end){
            return -1;
        }
        int middle = (start + end)/2;
        int middleData = data[middle];
        if(middleData == k){
            if((middle > 0 && data[middle - 1] != k) || middle == 0){
                return middle;
            }else{
               	end = middle - 1;
            }
        }
        else if(middleData > k){
            end = middle - 1;
        }else{
            start = middle + 1;
        }
        return GetFirstK(data, k, start, end);
    }
    int GetLastK(const vector<int> &data, int k, int start, int end){
        if(start > end){
            return -1;
        }
        int middle = (start + end)/2;
        int middleData = data[middle];
        if(middleData == k){
            if((middle < data.size() - 1 && data[middle + 1] != k) || middle == data.size() - 1){
                return middle;
            }else{
               	start = middle + 1;
            }
        }
        else if(middleData > k){
            end = middle - 1;
        }else{
            start = middle + 1;
        }
        return GetLastK(data, k, start, end);
    }
};
```

### 面试题53-2-0~n-1中缺失的数字

#### 题目描述

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0到n-1之内。在范围0到n-1的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

#### 分析

迭代二分查找，注意当缺失的数字出现在第一个和最后一个时的特殊处理

#### 代码

```c++
int GetMissingNumber(const int* numbers, int length)
{
    if(numbers == nullptr || length <= 0)
        return -1;

    int left = 0;
    int right = length - 1;
    while(left <= right)
    {
        int middle = (right + left) >> 1;
        if(numbers[middle] != middle)
        {
            if(middle == 0 || numbers[middle - 1] == middle - 1)//缺失的数字有可能是第一个
                return middle;
            right = middle - 1;
        }
        else
            left = middle + 1;
    }

    if(left == length)//缺失的数字是最后一个
        return length;

    // 无效的输入，比如数组不是按要求排序的，
    // 或者有数字不在0到n-1范围之内
    return -1;
}
```

### 面试题53-3-数组中数值和下标相等的元素

#### 题目描述

假设一个单调递增的数组里的每个元素都是整数并且是唯一的。请编程实现一个函数找出数组中任意一个数值等于其下标的元素。例如，在数组{-3, -1, 1, 3, 5}中，数字3和它的下标相等。

#### 分析

二分查找，搞清楚大于小于之后的查找方向就行了

#### 代码

```c++
int GetNumberSameAsIndex(const int* numbers, int length)
{
	if (numbers == nullptr || length <= 0) {
		return -1;
	}
	int left = 0;
	int right = length - 1;
	while (left <= right) {
		int middle = (left + right) >> 1;
		if (middle == numbers[middle]) {
			return middle;
		}
		else if(numbers[middle] > middle){
			right = middle - 1;
		}
		else {
			left = middle + 1;
		}
	}
	return -1;
}
```

