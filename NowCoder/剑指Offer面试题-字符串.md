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

请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

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