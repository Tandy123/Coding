## 【牛客】剑指Offer面试题

### 面试题43-1~n整数中1出现的次数

#### 题目描述

求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数

#### 分析

这题解题的关键在于从数字本身的规律着手，递归解决问题，时间复杂度为O(logn)

leetcode上的大神有种更巧妙的解法：[https://discuss.leetcode.com/topic/18054/4-lines-o-log-n-c-java-python](https://discuss.leetcode.com/topic/18054/4-lines-o-log-n-c-java-python)

他把数字每一位上1的次数独立出来计算，比如3141592，这时要求百位上的1出现的次数，先计算a=31415,b=92，这里的关键是a，当百位上是1时，前缀从“”到“3141”，一共有3142个情况，最后两位的情况有0-99这100种情况，这样就一共有3142\*100 = 314200种情况，这是对于当前位上的数字大于1的情况，当前位数字小于等于1时，入考虑同一个数字的千位，此时a=314,b=592，同理，虽然前缀从“”到“314”，貌似有315中情况，其实最后一中当前缀等于314时，千位上的1并没有把它所有的情况（1000中：从0-999）都遍历一遍，只遍历到了592，所以这时候b就派上了用场，这里的1出现了314\*1000 + 593 次，至此，每一位都可以按照这个规律进行计算，最终累加即可。

#### 代码
- 剑指Offer版：
```c
class Solution {
public:
    int NumberOf1Between1AndN_Solution(int n)
    {
    	if(n <= 0){
            return 0;
        }
        char strN[50];
        sprintf(strN, "%d",n);
        return NumberOf1(strN);
    }
    int NumberOf1(const char* strN){
        if(!strN || *strN < '0' || *strN >'9' || *strN == '\0'){
            return 0;
        }
        int first = *strN - '0';
        unsigned int length = static_cast<unsigned int>(strlen(strN));
        if(length  == 1 && first == 0){
            return 0;
        }
        if(length == 1 && first > 0){
            return 1;
        }
        int numFirstDigit = 0;
        if(first > 1){
            numFirstDigit = PowerBase10(length - 1);
        }else if(first == 1){
            numFirstDigit = atoi(strN+1) + 1;
        }
       	int numOtherDigits = first * (length - 1) * PowerBase10(length - 2);
        int numRecursive = NumberOf1(strN + 1);
        return numFirstDigit + numOtherDigits + numRecursive;
    }
    int PowerBase10(unsigned int n){
        int result = 1;
		for(int i = 0; i < n ; ++i){
            result *= 10;
        }
        return result;
    }
};
```
- Leetcoder大神版：
```c++
int countDigitOne(int n) {
    int ones = 0;
    for (long long m = 1; m <= n; m *= 10) {
        int a = n/m, b = n%m;
        ones += (a + 8) / 10 * m + (a % 10 == 1) * (b + 1);
    }
    return ones;
}
```