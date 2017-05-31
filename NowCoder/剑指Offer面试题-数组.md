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

### 面试题21-调整数组顺序使奇数位于偶数前面

#### 题目描述

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

#### 分析

比剑指Offer要求高一点，不然就解决这道题而言比较简单，双指针解决，这里还要考虑数字之间的相对位置不变  
**注意：算法的扩展性，可将奇偶判断和重排序解耦，使程序的功能得到扩展  
注意：类里面的函数指针要用静态的static修饰**


#### 代码

```c++
//***************剑指Offer里的要求****************
class Solution {
public:
	void reOrderArray(vector<int> &array) {
		reOrderArrayTemplate(array, isEven);
	}
	void reOrderArrayTemplate(vector <int> &array, bool (*fun)(int n)) {
		if (array.size() <= 0) {
			return;
		}
		int iBegin = 0;
		int iEnd = array.size() - 1;
		while (iBegin < iEnd) {
			while (iBegin < iEnd && !fun(array[iBegin])) {
				iBegin++;
			}
			while (iBegin < iEnd && fun(array[iEnd])) {
				iEnd--;
			}
			if (iBegin < iEnd) {
				swap(array[iBegin], array[iEnd]);
			}
		}
	}
	static bool isEven(int n) {
		if ((n & 1)== 0)
			return true;
		else
			return false;
	}
};

***********************牛客新要求***********************
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        reOrderArrayTemplate(array, isEven);
    }
    void reOrderArrayTemplate(vector <int> &array, bool (*fun)(int)){
        if(array.size() <= 0){
            return;
        }
        int oddCount = 0;
        vector<int> temp(array.size(), 0);
        for(int i = 0; i < array.size(); i++){
            if(!isEven(array[i])){
                oddCount++;
            }
        }
        int oddBegin = 0;
        for(int i = 0; i < array.size(); i++){
            if(!isEven(array[i])){
                temp[oddBegin++] = array[i];
            }else{
                temp[oddCount++] = array[i];
            }
        }
        for(int i = 0; i < array.size(); i++){
            array[i] = temp[i];
        }
    }
    static bool isEven(int n){
        if((n & 1) == 0)
            return true;
        else
            return false;
    }
};
```
