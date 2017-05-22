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