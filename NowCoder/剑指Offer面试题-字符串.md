## 【牛客】剑指Offer面试题-字符串

### 面试题5-替换空格

#### 题目描述

请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

#### 分析

从后往前替换

#### 代码
```c
class Solution {
public:
	void replaceSpace(char *str,int length) {
		if(str == nullptr || length <= 0)
        {
            return;
        }
           
        int originLength = 0;
        int numberOfBlank = 0;
        int i = 0;
        while(str[i] != '\0')
        {
            ++originLength;
            
            if(str[i] == ' ')
            {
                ++numberOfBlank;
            }
            
            ++i;
        }
        
        int newLength = originLength + numberOfBlank * 2;
        if(newLength > length)
        {
            return;
        }
        
        int indexOfOrigin = originLength;
        int indexOfNew = newLength;
        while(indexOfOrigin >= 0 && indexOfNew > indexOfOrigin)
        {
            if(str[indexOfOrigin] == ' ')
            {
                str[indexOfNew--] = '0';
                str[indexOfNew--] = '2';
                str[indexOfNew--] = '%';
            }
            else
            {
                str[indexOfNew--] = str[indexOfOrigin];
            }
            --indexOfOrigin;
        }
	}
};
```

### 面试题19-正则表达式匹配

#### 题目描述

请实现一个函数用来匹配包括'.'和'\*'的正则表达式。模式中的字符'.'表示任意一个字符，而'\*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab\*ac\*a"匹配，但是与"aa.a"和"ab\*a"均不匹配

#### 分析

分情况考虑

- 第二个字符不是*
- 第二个字符是*

#### 代码
```c
class Solution {
public:
    bool match(char* str, char* pattern)
    {
    	if(str == nullptr || pattern == nullptr)
        {
            return false;
        }
        return matchCore(str, pattern);
    }
  	bool matchCore(char *str, char *pattern)
    {
        if(*str ==  '\0' && *pattern == '\0')//if str and pattern both over
        {
            return true;
        }
        if(*str != '\0' && *pattern == '\0')//if str over and pattern does't over
        {
            return false;
        }
      	if(*(pattern + 1) == '*')//if the next character of pattern is *
        {
            if(*str ==  *pattern || (*pattern == '.' && *str != '\0'))
            //if current chars are match 
            //or the current char of pattern is . and str is not over
            {
                return matchCore(str + 1, pattern + 2)
                    || matchCore(str + 1, pattern)
                    || matchCore(str, pattern + 2);
            }
            else//if not match and pattern is not . or str is over
            {
                return matchCore(str, pattern + 2);
            }
        }
        else if(*str == *pattern || ((*pattern == '.' && *str != '\0')))
        {
            return matchCore(str + 1, pattern + 1);
        }
        return false;
    }
};
```

### 面试题20-表示数值的字符串

#### 题目描述

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

#### 分析

记住：数值的字符串遵循模式：A[.[B]][e|EC]或者.B[e|EC]，其中A为数值的整数部分，B紧跟着小数点为数值的小数部分，C紧跟着’e‘或者’E‘为数值的指数部分  
**注意：字符串传参的时候要改变指针指向时要传递地址，即双重指针**

#### 代码
```c++
class Solution {
public:
    bool isNumeric(char* string)
    {
        if(string == nullptr)
        {
            return false;
        }
        bool isnumeric = isInteger(&string);
        
        if(*string == '.'){
            ++string;
            isnumeric = isUnsignedInteger(&string) || isnumeric;
        }        
        if(*string == 'E' || *string =='e'){
            string++;
            isnumeric = isInteger(&string) && isnumeric;
        }
        if(*string == '\0' && isnumeric){
            return true;
        }else{
            return false;
        }
    }
	bool isUnsignedInteger(char **string)
    {
        int flag = 0;
        while(**string != '\0' && **string >= '0' && **string <= '9'){
            (*string)++;
            flag++;
        }
        return flag > 0;
    } 
    bool isInteger(char **string)
    {
        if(**string == '+' || **string == '-'){
            (*string)++;
        }
        return isUnsignedInteger(string);
    }
};
```