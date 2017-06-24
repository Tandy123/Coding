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

### 面试题38-字符串的排列

#### 题目描述

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

输入描述:  
输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

#### 分析

和书上的有点不同，这里的输入字符可能重复，并且要求按照字典序输出，重点看一下第二个版本的代码，通过传值自动达到了按照字典序输出的要求

#### 代码
- 按书本上的思路来，考虑了字符重复的情况
```c++
class Solution {
public:
    vector<string> Permutation(string str) {
       	vector<string> res;
        if(str.length() == 0){
            return res;
        }
        Permutation(str, 0, res);
        sort(res.begin(), res.end());
        return res;
    }
    void Permutation(string &str, int begin, vector<string> &res){
        if(begin >= str.length()){
            res.push_back(str);
        }else{
            for(int i = begin; i < str.length(); ++i){
                if(begin != i && str[begin]==str[i]){
                    continue;
                }else{
                    swap(str[begin],str[i]);
                	Permutation(str, begin+1, res);
                	swap(str[begin],str[i]);
                }
            }
        }
    }
};
```
- 下面这个更简洁一点，把最后的排序去掉放到了最开始，传引用改为传值按书本上的思路来，考虑了字符重复的情况
```c++
class Solution {
public:
    vector<string> Permutation(string str) {
       	vector<string> res;
        if(str.length() == 0){
            return res;
        }
sort(str.begin(), str.end());
        Permutation(str, 0, res);
        //sort(res.begin(), res.end());
        return res;
    }
    void Permutation(string str, int begin, vector<string> &res){
        if(begin >= str.length()){
            res.push_back(str);
        }else{
            for(int i = begin; i < str.length(); ++i){
                if(begin != i && str[begin]==str[i]){
                    continue;
                }else{
                    swap(str[begin],str[i]);
                	Permutation(str, begin+1, res);
                	//swap(str[begin],str[i]);
                }
            }
        }
    }
};
```

### 面试题38-2拓展-字符串的组合

#### 题目描述
输入一个字符串，打印出该字符串中字符的所有组合。例如输入字符串abc，则打印出由字符a、b、c所能组合出来的所有字符串a、b、c、ab、ac、bc和abc。

#### 分析

递归实现，去重操作可放在左右调用stl进行

#### 代码
- 按书本上的思路来，考虑了字符重复的情况
```c++
class Solution {
public:
    void Combination(char *string)
	{
		if (string == NULL) {
			return;
		}
		vector<char> result;
		int i, length = strlen(string);
		for (i = 1; i <= length; ++i)
			Combination(string, i, result);
	}
	
	void Combination(char *string, int number, vector<char> &result)
	{
		//assert(string != NULL);
		if (number == 0)
		{
			static int num = 1;
			printf("第%d个组合\t", num++);
	
			vector<char>::iterator iter = result.begin();
			for (; iter != result.end(); ++iter)
				printf("%c", *iter);
			printf("\n");
			return;
		}
		if (*string == '\0')
			return;
		result.push_back(*string);
		Combination(string + 1, number - 1, result);
		result.pop_back();
		Combination(string + 1, number, result);
	}
};
```

### 面试题58-1-翻转单词顺序

#### 题目描述

牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

#### 分析
- 书上的思路，先整体翻转，然后以空格为间隔，单独翻转单词
- 牛客上的大神，直接一个一个单词处理，后面读的单词加到前面来

#### 代码
- 剑指Offer版
```c++
class Solution {
public:
    string ReverseSentence(string str) {
        if(str.length() <= 1){
            return str;
        }
        Reverse(str, 0, str.length()-1);
        int begin = 0; 
        int end = 0;
        while(begin < str.length()){
            if(str[begin] == ' '){
                ++begin;
                ++end;
            }else if(str[end] == ' ' || end == str.length()){
                Reverse(str, begin, end - 1);
                begin = end;
            }else{
                ++end;
            }
        }
        return str;
    }
    void Reverse(string &str, int begin, int end){
        if(begin == end){
            return;
        }
        while(begin < end){
            char temp = str[begin];
            str[begin] = str[end];
            str[end] = temp;
            begin++;
            end--;
        }
    }
};
```
- 牛客大神版
```c++
class Solution {
public:
    string ReverseSentence(string str) {
        string res = "", tmp = "";
        for(unsigned int i = 0; i < str.size(); ++i){
            if(str[i] == ' ') res = " " + tmp + res, tmp = "";
            else tmp += str[i];
        }
        if(tmp.size()) res = tmp + res;
        return res;
    }
}; 
```

### 面试题58-2-左旋转字符串

#### 题目描述
汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

#### 分析
利用上一题的翻转函数，通过三次翻转解决，同时利用内置的reverse函数也可以做到

#### 代码
- 剑指Offer版（自己实现Reverse翻转函数）
```c++
class Solution {
public:
    string LeftRotateString(string str, int n) {
        if(str.length() <= 1 || n <= 0){
            return str;
        }
        Reverse(str, 0, n - 1);
        Reverse(str, n, str.length() - 1);
        Reverse(str, 0, str.length() - 1);
        return str;
    }
    void Reverse(string &str, int begin, int end){
        if(begin >= end){
            return;
        }
        while(begin < end){
            char temp = str[begin];
            str[begin] = str[end];
            str[end] = temp;
            ++begin;
            --end;
        }
    }
};
```
- 调用reverse库函数
```c++
class Solution {
public:
    string LeftRotateString(string str, int n) {
        reverse(str.begin(),str.begin()+n);
        reverse(str.begin()+n,str.end());
        reverse(str.begin(),str.end());
        return str;  
    }
};
```