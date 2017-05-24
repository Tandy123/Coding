## 【牛客】剑指Offer面试题-查找和排序

### 快速排序

#### 思想

实现快速排序的关键在于现在数组中选择一个数字，接下来把数组中的数字分成两部分，比选择的数字小的数字移到数组左边，比选择的数字大的数字移到数组的右边。

#### 代码

```c++
int partition(vector<int> &nums, int low, int high) {//每次都用第一个作为基准元素，有序的情况下退化为n^2
	int key = nums[low]; //基准元素  
	while (low < high) { //从表的两端交替地向中间扫描  
		while (low < high  && nums[high] >= key)
			--high; //从high 所指位置向前搜索，至多到low+1 位置。将比基准元素小的交换到低端 
		swap(nums[low], nums[high]);
		while (low < high  && nums[low] <= key) ++low;
		swap(nums[low], nums[high]);
	}
	print(nums, step++);
	return low;
}

int partitionII(vector<int> &nums, int low, int high) {//【剑指Offer】每次随机找一个基准元素
	if (nums.size() == 0 || low <0 || high <= low || high > nums.size() - 1)
		throw new std::exception("Invalid Parameters");
	
	int index = low + rand()%(high - low + 1);
	swap(nums[index], nums[high]);
	int small = low-1;
	for (int index = low; index < high; ++index)
	{
		if(nums[index] < nums[high])
		{
			small++;
			if(small != index)
			{
				swap(nums[small], nums[index]);
			}
		}
	}
	small++;
	swap(nums[small], nums[high]);
	return small;
}
void quickSort(vector<int> &nums, int low, int high) {
	if (low < high) {
		int loc = partitionII(nums, low, high);  //将表一分为二  
		quickSort(nums, low, loc - 1);          //递归对低子表递归排序  
		quickSort(nums, loc + 1, high);        //递归对高子表递归排序  
	}
}
//快速排序
void QuickSort(vector<int> &nums) {
	cout << "Quick Sort: " << endl;
	quickSort(nums, 0, nums.size() - 1);//递归实现
}
```

### 面试题11-旋转数组的最小数字

#### 题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。
例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。
NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

#### 分析

二分查找，考虑特殊情况，当前中后指向的数字相同时，回归顺序查找最小数的算法

#### 代码
```c
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        if(rotateArray.size() == 0)
            return 0;
        int index1 = 0;
        int index2 = rotateArray.size()-1;
        int indexMid = index1;
        while(rotateArray[index1] >= rotateArray[index2])
        {
 			// 如果index1和index2指向相邻的两个数，
     		// 则index1指向第一个递增子数组的最后一个数字，
        	// index2指向第二个子数组的第一个数字，也就是数组中的最小数字
            if(index1 + 1 == index2)
            {
                indexMid = index2;
                break;
            }
            indexMid = (index1 + index2)/2;
			// 如果下标为index1、index2和indexMid指向的三个数字相等，则只能顺序查找
            if(rotateArray[index1] == rotateArray[index2] && 
              rotateArray[index1] == rotateArray[indexMid])
            {
                return MinInOrder(rotateArray, index1, index2);
            }
            else if(rotateArray[indexMid] >= rotateArray[index1])//这里一定要考虑等号的情况
            {
                index1 = indexMid;
            }
            else if(rotateArray[indexMid] <= rotateArray[index2])//这里一定要考虑等号的情况
            {
                index2 = indexMid;
            }
        }
        return rotateArray[indexMid];
    }
    int MinInOrder(vector<int> &rotateArray, int index1, int index2)
    {
        if(rotateArray.size() == 0)
        {
            return 0;
        }
        int res = rotateArray[index1];
        for(int i = index1+1; i <= index2; i++)
        {
            if(rotateArray[i] < res)
            {
                res = rotateArray[i];
            }
        }
        return res;
    }
};
```
