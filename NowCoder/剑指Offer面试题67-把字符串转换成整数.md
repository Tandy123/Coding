## 【牛客】剑指Offer面试题67

### 面试题67-把字符串转换成整数

#### 题目描述
将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0 

#### 输入描述:
输入一个字符串,包括数字字母符号,可以为空
#### 输出描述:
如果是合法的数值表达则返回该数字，否则返回0
#### 输入例子:
	+2147483647
    1a33
#### 输出例子:
	2147483647
    0

#### 分析

要考虑一下几点：

  1. 当非法输入出现时，用全局变量来记录
  2. 考虑字符串为空和空指针
  3. 考虑正负号，以及只有正负号的情况
  4. 考虑出现出了数字之外的字符
  5. 考虑上下溢出

#### 代码
- 剑指Offer版
```c++
enum Status{kValid = 0, kInvalid};
int g_nStatus = kValid;
class Solution {
public:
    int StrToInt(string str) {
        g_nStatus = kInvalid;
        long long num = 0;
        if(str.size() > 0){
            bool minus = false;
            int i = 0;
            if(str[i] == '+'){
                i++;
            }else if(str[i] == '-'){
                minus = true;
                i++;
            }
            if(i < str.size()){
                num = StrToIntCore(str, i, minus);
            }
        }
        return (int)num;
    }
    long long StrToIntCore(string &str, int i, bool minus){
        long long num = 0;
        while(i < str.size()){
            if(str[i] >= '0' && str[i]<='9'){
                int flag = minus?-1:1;
                num = num * 10 + flag * (str[i] - '0');
                if((!minus && num > 0x7FFFFFFF) || (minus && num < (signed int)0x80000000)){
                    num = 0;
                    break;
                }else{
                    i++;
                }
            }else{
                num = 0;
                break;
            }
        }
        if(i == str.size()){
            g_nStatus = kValid;
        }
        return num;
    }
};
```
