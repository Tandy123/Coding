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


### 面试题39-数组中出现超过一半的数字

#### 题目描述

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

#### 分析

- 最直接：
	- 建议数字和次数的map，时间O(n)，空间O(n)
- 排序：
	- 先快排序整体排序，找中间的值，最终验证，时间O(nlogn)，空间O(1)
	- 利用快排思想，借鉴partition，不断二分找到中间的值，最终验证，时间O(n)，空间O(1)
- 最优：
	- 根据正负抵消原理，超过一半的数至少可以抵消掉其他所的数，最终验证，时间O(n)，空间O(1)


#### 代码
- 思路1（略）
- 思路2
```c++
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
    	bool isInputInvalid = false;
        if(numbers.size() <= 0){
            isInputInvalid = true;
            return 0;
        }
        int start = 0;
        int end = numbers.size()-1;
        int middle = Partition(numbers, start, end);
        while(middle != numbers.size()/2){
         	if(middle < numbers.size()/2) {
                start = middle + 1;
                middle = Partition(numbers, start, end);
            }  else{
                end = middle - 1;
                middle = Partition(numbers, start, end);
            }
        }
     	int res = numbers[middle];
       	if(!CheckMoreThanHalf(numbers, res)){
            isInputInvalid = true;
            res = 0;
        }
        return res;
    }
    int Partition(vector<int> &numbers, int start, int end){
        if(end == start){
            return end;
        }
        int index = rand()%(end - start) + start;
        swap(numbers[index], numbers[end]);
        int small = start - 1;
        for(index = start; index <= end; ++index){
            if(numbers[index] <= numbers[end]){
                small++;
                if(small != index){
                    swap(numbers[index], numbers[small]);
                }
            }
        }
        return small;
    }
    bool CheckMoreThanHalf(const vector<int> &numbers, const int res){
        int times = 0;
        for(int i = 0; i < numbers.size(); ++i){
            if(numbers[i] == res){
                ++times;
            }
        }
        return 2 * times > numbers.size()?true:false; 
    }
};
```
- 思路3
```c++
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
    	bool isInputInvalid = false;
        if(numbers.size() <= 0){
            isInputInvalid = true;
            return 0;
        }
        int res = numbers[0];
        int times = 1;
        for(int i = 1; i < numbers.size(); ++i){
            if(times == 0){
                res = numbers[i];
                times = 1;
            }else{
                if(numbers[i] == res){
                    ++times;
                }else{
                    --times;
                }
            }
        }
       	if(!CheckMoreThanHalf(numbers, res)){
            isInputInvalid = true;
            res = 0;
        }
        return res;
    }
    bool CheckMoreThanHalf(const vector<int> &numbers, const int res){
        int times = 0;
        for(int i = 0; i < numbers.size(); ++i){
            if(numbers[i] == res){
                ++times;
            }
        }
        return 2 * times > numbers.size()?true:false; 
    }
};
```

### 面试题40-最小的K个数

#### 题目描述
输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

#### 分析

- 思路1：
利用快排的Partition函数找到第K大的数，时间O(n)，问题是会改变原有的数组
- 思路2：
利用红黑树结构的容器set实现O(1)时间找到k个数中的最大数，进行替换并维护一个最大堆，总的时间复杂度为O(nlogk)，这里注意set的使用


#### 代码
- 思路1
```c++
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> res;
        if(k <= 0 || input.size() == 0 || input.size() < k){
            return res;
        }
        int start = 0;
        int end = input.size() - 1;
        int index = Partition(input, start, end);
        while(index != k - 1){
            if(index < k - 1){
                start = index + 1;
                index = Partition(input, start, end);
            }else{
                end = index - 1;
                index = Partition(input, start, end);
            }
        }
        for(int i = 0; i < k && i < input.size(); ++i){
            res.push_back(input[i]);
        }
        return res;
    }
    int Partition(vector<int> &input, int start, int end){
        if(start == end){
            return start;
        }
        int index = rand()%(end - start) + start;
        swap(input[index], input[end]);
        int small = start - 1;
        for(index = start; index <= end; ++index){
            if(input[index] <= input[end]){
                small++;
                if(small != index){
                    swap(input[small], input[index]);
                }
            }
        }
        return small;
    }
};
```
- 思路2
```c++
class Solution {
public:
    typedef multiset<int, greater<int>> intSet;
    typedef multiset<int, greater<int>>::iterator setIterator;
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> res;
        intSet leastNumbers;
        if(k <= 0 || input.size() == 0 || input.size() < k){
            return res;
        }
        vector<int>::const_iterator iter = input.begin();
        for(; iter!= input.end(); ++iter){
            if(leastNumbers.size()<k){
                leastNumbers.insert(*iter);
            }else{
                setIterator iterGreatest = leastNumbers.begin();
                if(*iter < *(leastNumbers.begin())){
                    leastNumbers.erase(iterGreatest);
                    leastNumbers.insert(*iter);
                }
            }
        }
        setIterator iterG = leastNumbers.begin();
        for(; iterG!= leastNumbers.end(); ++iterG){
            res.push_back(*iterG);
        }
        return res;
    }
};
```