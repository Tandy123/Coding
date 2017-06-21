## 【牛客】剑指Offer面试题-动态规划与贪婪算法

### 动态规划的四个特点

1. 目标是求一个问题的最优解；
2. 整体问题的最优解是依赖各个子问题的最优解；
3. 把大问题分解成若干个小问题，这些小问题之间还有相互重叠的更小的问题；
4. 从上往下分析问题，从下往上求解问题。

### 面试题14-剪绳子

#### 题目描述

给你一根长度为n绳子，请把绳子剪成m段（m、n都是整数，n>1并且m≥1）。每段的绳子的长度记为k[0]、k[1]、……、k[m]。k[0] \* k[1] \* … \* k[m]可能的最大乘积是多少？例如当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到最大的乘积18。


#### 分析

两种思路：

动态规划（时间O(n^2)，空间O(n)）：  
把从4到length的所有最优解都存在一个数组里，用子问题的最优解推出最终的最优解  

贪婪（时间O(1)，空间O(1)）：  
当n>=5时，每次尽可能多地剪长度为3的绳子，当剩下的绳子长度为4时，把绳子剪成长度为2的两段。  
数学证明如下：  
首先，当n>=5时，有2(n-2)>n和3(n-3)>n；  
另外，当n>=5时，3(n-2)>=2(n-2)；  
因此，我们要尽可能地多剪长度为3的绳子。  

#### 代码
```c
// ====================动态规划====================
int maxProductAfterCutting_solution1(int length)
{
    if(length < 2)
        return 0;
    if(length == 2)
        return 1;
    if(length == 3)
        return 2;

    int* products = new int[length + 1];
    products[0] = 0;
    products[1] = 1;
    products[2] = 2;
    products[3] = 3;

    int max = 0;
    for(int i = 4; i <= length; ++i)
    {
        max = 0;
        for(int j = 1; j <= i / 2; ++j)
        {
            int product = products[j] * products[i - j];
            if(max < product)
                max = product;

            products[i] = max;
        }
    }

    max = products[length];
    delete[] products;

    return max;
}

// ====================贪婪算法====================
int maxProductAfterCutting_solution2(int length)
{
    if(length < 2)
        return 0;
    if(length == 2)
        return 1;
    if(length == 3)
        return 2;

    // 尽可能多地减去长度为3的绳子段
    int timesOf3 = length / 3;

    // 当绳子最后剩下的长度为4的时候，不能再剪去长度为3的绳子段。
    // 此时更好的方法是把绳子剪成长度为2的两段，因为2*2 > 3*1。
    if(length - timesOf3 * 3 == 1)
        timesOf3 -= 1;

    int timesOf2 = (length - timesOf3 * 3) / 2;

    return (int) (pow(3, timesOf3)) * (int) (pow(2, timesOf2));
}
```


### 面试题46-把数字翻译成字符串

#### 题目描述

给定一个数字，我们按照如下规则把它翻译为字符串：0翻译成"a"，1翻译成"b"，……，11翻译成"l"，……，25翻译成"z"。一个数字可能有多个翻译。例如12258有5种不同的翻译，它们分别是"bccfi"、"bwfi"、"bczi"、"mcfi"和"mzi"。请编程实现一个函数用来计算一个数字有多少种不同的翻译方法。


#### 分析

动态规划：f(i)表示到第i位有多少种翻译方法，其中f(0)=1,当i>1时，f(i)=f(i-1) + g(i-1,i)f(i-2), g(i-1, i)表示第i-1位和第i位结合起来的数字是否在10-25的范围内，注意要大于10，不然像00，09这样的表示是不存在的，直接,0，9就行了。

书上的思路也是动态规划，只是遍历的时候是从最后一位开始

#### 代码
- 我的代码：
```c
int GetTranslationCount(int number)
{
	if (number < 0)
		return 0;

	string numberInString = to_string(number);
	return GetTranslationCount(numberInString);
}

int GetTranslationCount(const string& number)
{
	int length = number.length();
	int* counts = new int[length];
	int count = 0;

	counts[0] = 1;

	for (int i = 1; i < length; ++i) {
		counts[i] = counts[i - 1];
		string s2 = number.substr(i - 1, 2);
		int n2 = atoi(s2.c_str());
		
		if (n2 > 9 && n2 < 26) {
			if (i - 2 >= 0)
			{
				counts[i] += counts[i - 2];
			}
			else {
				counts[i] += 1;
			}
		}
		
	}
	count = counts[length - 1];
	delete[] counts;

	return count;
}
```
- 书上的代码：
```c++
int GetTranslationCount(int number)
{
    if(number < 0)
        return 0;

    string numberInString = to_string(number);
    return GetTranslationCount(numberInString);
}

int GetTranslationCount(const string& number)
{
    int length = number.length();
    int* counts = new int[length];
    int count = 0;

    for(int i = length - 1; i >= 0; --i)
    {
        count = 0;
         if(i < length - 1)
               count = counts[i + 1];
         else
               count = 1;

        if(i < length - 1)
        {
            int digit1 = number[i] - '0';
            int digit2 = number[i + 1] - '0';
            int converted = digit1 * 10 + digit2;
            if(converted >= 10 && converted <= 25)
            {
                if(i < length - 2)
                    count += counts[i + 2];
                else
                    count += 1;
            }
        }

        counts[i] = count;
    }

    count = counts[0];
    delete[] counts;

    return count;
}
```


### 面试题47-礼物的最大值

#### 题目描述

在一个m×n的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向左或者向下移动一格直到到达棋盘的右下角。给定一个棋盘及其上面的礼物，请计算你最多能拿到多少价值的礼物？


#### 分析

动态规划，不难，注意可以把矩阵优化成一维的

#### 代码
- 二维矩阵动态规划
```c
int getMaxValue_solution1(const int* values, int rows, int cols)
{
	if (values == nullptr || rows <= 0 || cols <= 0) {
		return 0;
	}
	int** maxValues = new int*[rows];
	for (int i = 0; i < rows; ++i) {
		maxValues[i] = new int[cols];
	}
	for (int row = 0; row < rows; ++row) {
		for (int col = 0; col < cols; ++col) {
			int up = 0;
			int left = 0;
			if (row > 0) {
				up = maxValues[row - 1][col];
			}
			if (col > 0) {
				left = maxValues[row][col - 1];
			}
			maxValues[row][col] = std::max(up, left) + values[row * cols + col];
		}
	}
	int maxValue = maxValues[rows - 1][cols - 1];
	for (int i = 0; i < rows; ++i)
		delete[] maxValues[i];
	delete[] maxValues;

	return maxValue;
}
```
- 一维矩阵动态规划：
```c++
int getMaxValue_solution2(const int* values, int rows, int cols)
{
	if (values == nullptr || rows <= 0 || cols <= 0) {
		return 0;
	}
	int* maxValues = new int[cols];
	for (int row = 0; row < rows; ++row) {
		for (int col = 0; col < cols; ++col) {
			int up = 0;
			int left = 0;
			if (row > 0) {
				up = maxValues[col];
			}
			if (col > 0) {
				left = maxValues[col - 1];
			}
			maxValues[col] = std::max(up, left) + values[row * cols + col];
		}
	}
	int maxValue = maxValues[cols - 1];
	delete[] maxValues;

	return maxValue;
	return 0;
}
```

### 面试题48-最长不含重复字符的子字符串

#### 题目描述
请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。假设字符串中只包含从'a'到'z'的字符。

#### 分析
蛮力法就算了，直接上动态规划，f(i)表示以第i个字符为结尾的不包含重复字符的子字符串的最大长度。这里记录字符最近一次出现过的位置的方法是借鉴了映射的思想

#### 代码
```c
int longestSubstringWithoutDuplication_2(const std::string& str)
{
    int curLength = 0;
    int maxLength = 0;

    int* position = new int[26];
    for(int i = 0; i < 26; ++i)
        position[i] = -1;

    for(int i = 0; i < str.length(); ++i)
    {
        int prevIndex = position[str[i] - 'a'];
        if(prevIndex < 0 || i - prevIndex > curLength)
            ++curLength;
        else
        {
            if(curLength > maxLength)
                maxLength = curLength;

            curLength = i - prevIndex;
        }
        position[str[i] - 'a'] = i;
    }

    if(curLength > maxLength)
        maxLength = curLength;

    delete[] position;
    return maxLength;
}
```