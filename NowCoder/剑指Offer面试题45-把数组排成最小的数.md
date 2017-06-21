## 【牛客】剑指Offer面试题

### 面试题43-1~n整数中1出现的次数

#### 题目描述

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

#### 分析

将数组排序时候的比较规则必须拼在一起之后比较，ab<ba，比较函数要是static的

牛客上面没有考虑大数问题，书上提到了用字符串来存储数字

#### 代码
- 没有考虑大数的情况
```c
class Solution {
public:
    string PrintMinNumber(vector<int> numbers) {
        sort(numbers.begin(), numbers.end(),cmp);
        string res = "";
        for(int i = 0; i < numbers.size(); ++i){
            res += to_string(numbers[i]);
        }
        return res;
    }
    static bool cmp(int a, int b){
        string sa = to_string(a);
        string sb = to_string(b);
        return sa+sb < sb+sa;
    }
};
```
- 考虑了大数的情况
```c++
class Solution {
public:
    string PrintMinNumber(vector<int> numbers) {
        vector<string> sv(numbers.size(), "");
        for(int i = 0; i < numbers.size(); ++i){
            sv[i] = to_string(numbers[i]);
        }
        sort(sv.begin(), sv.end(),cmp);
        string res = "";
        for(int i = 0; i < numbers.size(); ++i){
            res += sv[i];
        }
        return res;
    }
    static bool cmp(string a, string b){
        return a+b < b+a;
    }
};
```