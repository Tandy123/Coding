## 【牛客】剑指Offer面试题

### 面试题43-1~n整数中1出现的次数

#### 题目描述

数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从0开始计数）是5，第13位是1，第19位是4，等等。请写一个函数求任意位对应的数字。

#### 分析

这道题的解法是将数字按照数位的个数进行分段，然后逐段排查，时间复杂度为O(n)，这里n表示要找的数的位数。

#### 代码
```c
#include <iostream>
#include <algorithm>

using namespace std;

int countOfInteger(int digits) {
	if (digits == 1) {
		return 10;
	}
	else {
		return (int)pow(10, digits-1) * 9;
	}
}

int beginNumber(int digits) {
	if (digits == 1) {
		return 0;
	}
	else {
		return (int)pow(10, digits - 1);
	}
}

int digitAtIndex(int digits, int index){
	int number = beginNumber(digits) + index/digits;
	int indexFromRight = index % digits;
	return (number / (int)pow(10, digits - indexFromRight - 1))% 10;
}

int digitAtIndex(int index) {
	if (index < 0) {
		return -1;
	}
	int digits = 1;
	while (true) {
		int numbers = countOfInteger(digits);
		if (numbers * digits > index) {
			return digitAtIndex(digits, index);
		}
		index -= numbers * digits;
		++digits;
	}
	return -1;
}

void test(const char* testName, int inputIndex, int expectedOutput)
{
	if (digitAtIndex(inputIndex) == expectedOutput)
		cout << testName << " passed." << endl;
	else
		cout << testName << " FAILED." << endl;
}

int main()
{
	test("Test1", 0, 0);
	test("Test2", 1, 1);
	test("Test3", 9, 9);
	test("Test4", 10, 1);
	test("Test5", 189, 9);  // 数字99的最后一位，9
	test("Test6", 190, 1);  // 数字100的第一位，1
	test("Test7", 1000, 3); // 数字370的第一位，3
	test("Test8", 1001, 7); // 数字370的第二位，7
	test("Test9", 1002, 0); // 数字370的第三位，0
	return 0;
}
```