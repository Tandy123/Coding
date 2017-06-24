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

### 面试题41-数据流中的中位数

#### 题目描述
如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

#### 分析

- 利用最大堆和最小堆实现：
	- 数据数量为偶数时，最大堆和最小堆里数据一样多;数据数量为奇数时，让最大堆中多一个数。
	- 通过三次调整堆的操作保证最小堆的数都小于最大堆的数
- 难点还在于heap相关的数据结构的使用

#### 代码
- 剑指Offer版
```c++
class Solution {
public:
    void Insert(int num)
    {
        if(((min.size()+max.size())&1) == 0){
            if(max.size() > 0 && num < max[0]){
                max.push_back(num);
                push_heap(max.begin(), max.end(), less<int>());
                num = max[0];
                pop_heap(max.begin(), max.end(), less<int>());
                max.pop_back();
            }
            min.push_back(num);
            push_heap(min.begin(), min.end(), greater<int>());
        }else{
            if(min.size() > 0 && num > min[0]){
                min.push_back(num);
                push_heap(min.begin(), min.end(), greater<int>());
                num = min[0];
                pop_heap(min.begin(), min.end(), greater<int>());
                min.pop_back();
            }
            max.push_back(num);
            push_heap(max.begin(), max.end(), less<int>());
        }
    }

    double GetMedian()
    { 
        int size = min.size() + max.size();
        if(size == 0){
            return 0;
        }
    	if((size&1) == 1){
            return min[0];
        }else{
            return (min[0] + max[0])/2.0;
        }
    }
private:
    vector<int> min;
    vector<int> max;
};
```
- 牛客大神版（用了priority_queue）
```c++
class Solution {
    priority_queue<int, vector<int>, less<int> > p;
    priority_queue<int, vector<int>, greater<int> > q;
     
public:
    void Insert(int num){
        if(p.empty() || num <= p.top()) p.push(num);
        else q.push(num);
        if(p.size() == q.size() + 2) q.push(p.top()), p.pop();
        if(p.size() + 1 == q.size()) p.push(q.top()), q.pop();
    }
    double GetMedian(){ 
      return p.size() == q.size() ? (p.top() + q.top()) / 2.0 : p.top();
    }
};
```

### 面试题42-连续子数组的最大和

#### 题目描述
HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。你会不会被他忽悠住？(子向量的长度至少是1)

#### 分析

- 书中列举了举例分析和动态规划两种方法，其实异曲同工的，都是忽略掉之前的和为负数的情况，每次更新全局最大值

#### 代码
```c++
class Solution {
public:
    bool InvalidInput = false;
    int FindGreatestSumOfSubArray(vector<int> array) {
    	if(array.size() == 0){
            InvalidInput = true;
            return 0;
        }
        int nCurSum = 0;
        int nGreatestSum = 0x80000000;//32位的int，正数的范围是(0,0x7FFFFFFF),负数(0x80000000,0xFFFFFFFF)
        for(int i = 0; i < array.size(); ++i){
            if(nCurSum <= 0){
                nCurSum = array[i];
            }else{
                nCurSum += array[i];
            }
            if(nCurSum > nGreatestSum){
                nGreatestSum = nCurSum;
            }
        }
        return nGreatestSum;
    }
};
```

### 面试题56-1-数组中数字出现的次数

#### 题目描述
一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

#### 分析
用异或来做：两个相同的数字异或得到0，两个不同的数字异或一定存在某位为1，最巧妙的地方在于作者利用这两个性质对数组进行分组。

#### 代码
```c++
class Solution {
public:
    void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) {
		if(data.size()  <= 1){
            return;
        }
        int resultXOR = 0;
        for(int i = 0; i < data.size(); ++i){
            resultXOR ^= data[i];
        }
        unsigned int indexOf1 = FindFirstBitIs1(resultXOR);
        *num1 = 0;
        *num2 = 0;
        for(int i = 0; i < data.size(); ++i){
            if(IsBit1(data[i], indexOf1)){
                *num1 ^= data[i];
            }else{
                *num2 ^= data[i];
            }
        }
        return;
    }
    unsigned int FindFirstBitIs1(int num){
        int indexOf1 = 0;
        while(((num & 1) == 0) && indexOf1 < 8 * sizeof(int)){
            num = num>>1;
            ++indexOf1;
        }
        return indexOf1;
    }
    bool IsBit1(int num, unsigned int bit){
        num = num >> bit;
        return num & 1;
    }
};
```

### 面试题56-2-数组中唯一只出现一次的数字

#### 题目描述
在一个数组中除了一个数字只出现一次之外，其他数字都出现了三次。请找出那个吃出现一次的数字。

#### 分析
由于这次的重复次数是三，三个相同的数字异或还是本身，所以不能用异或来做，可以把二进制下每一位的数字相加，如果某一位的总和是三的整数倍，则要求的数字在该位上必为0，否则为1

#### 代码
```c++
int FindNumberAppearingOnce(int numbers[], int length)
{
    if(numbers == nullptr || length <= 0)
        throw new std::exception("Invalid input.");

    int bitSum[32] = {0};
    for(int i = 0; i < length; ++i)
    {
        int bitMask = 1;
        for(int j = 31; j >= 0; --j)
        {
            int bit = numbers[i] & bitMask;
            if(bit != 0)
                bitSum[j] += 1;

            bitMask = bitMask << 1;
        }
    }

    int result = 0;
    for(int i = 0; i < 32; ++i)
    {
        result = result << 1;
        result += bitSum[i] % 3;
    }

    return result;
}
```

### 面试题57-1-和为s的两个数字

#### 题目描述
输入一个递增排序的数组和一个数字S，在数组中查找两个数，是的他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。 

#### 输出描述:
对应每个测试案例，输出两个数，小的先输出。

#### 分析
设置头尾两个指针，进行夹逼，前提条件：有序

#### 代码
```c++
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        vector<int> result;
        if(array.size() <= 0){
            return result;
        }
        int ahead = 0;
        int behind = array.size() - 1;
        while(ahead < behind){
            int curSum = array[ahead] + array[behind];
            if(curSum > sum){
                --behind;
            }else if(curSum < sum){
                ++ahead;
            }else{
                result.push_back(array[ahead]);
                result.push_back(array[behind]);
                break;
            }
        }
        return result;
    }
};
```

### 面试题57-2-和为s的连续正数序列

#### 题目描述
小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck! 

#### 输出描述:
输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

#### 分析
两种思路
- 思路一：剑指Offer上的，设置大小两个指针，计算当前的两个指针之间的数字和，和大了小指针前进，和小了大指针前进，知道小指针指向的数字超过(sum+1)/2为止
- 思路二：根据求和公式：(start + end)(end - start + 1) = sum，直接求解一元二次方程组(start + end)(end - start + 1)=2*sum=k*l(k>l)，end=(k+l-1)/2  start =(k-l+1)/2，求出所有满足条件的整数解

#### 代码
- 思路一
```c++
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int>> res;
        if(sum < 3){
            return res;
        }
        int small = 1;
        int big = 2;
        int middle = (sum+1) / 2;
        int curSum = small + big;
        while(small < middle){
            if(curSum == sum){
                vector<int> temp;
                for(int i = small; i <= big; ++i){
                    temp.push_back(i);
                }
                res.push_back(temp);
            }
            while(curSum > sum && small < middle){
                curSum -= small;
                ++small;
                if(curSum == sum){
                    vector<int> temp;
                    for(int i = small; i <= big; ++i){
                        temp.push_back(i);
                    }
                    res.push_back(temp);
                }
            }
            ++big;
            curSum += big;
        }
        return res;
    }
};
```
- 思路二
```c++
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int>> res;
        if(sum < 3){
            return res;
        }
        int s = sqrt(2*sum);
        for(int i = s; i >= 2; --i){
            if((2*sum) % i == 0){
                int d = (2*sum) / i;
                if(((d & 1) == 0 && (i & 1)==1)||((d & 1) == 1 && (i & 1)==0)){
                    int start = (d - i + 1)/2;
                    int end = (d + i - 1)/2;
                    vector<int> temp;
                    for(int i = start; i <= end; ++i){
                        temp.push_back(i);
                    }
                    res.push_back(temp);
                }
            }
        }
        return res;
    }
};
```