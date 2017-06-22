## 【牛客】剑指Offer面试题-空间换时间

### 面试题50-1-字符串中第一个只出现一次的字符

#### 题目描述

在一个字符串(1<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置

#### 分析

空间换时间：利用哈希表

#### 代码
- 用vector实现
```c
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        if(str.length() <= 0){
            return -1;
        }
        vector<int>hashTable(256, 0);
        for(int i = 0; i < str.length(); ++i){
            hashTable[(int)str[i]]++;
        }
        for(int i = 0; i < str.length(); ++i){
            if(hashTable[(int)str[i]] == 1){
                return i;
            }
        }
        return -1;
    }
};
```
- 用map实现
```c++
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        if(str.length() <= 0){
            return -1;
        }
        map<char, int>m;
        for(int i = 0; i < str.length(); ++i){
            m[(int)str[i]]++;
        }
        for(int i = 0; i < str.length(); ++i){
            if(m[(int)str[i]] == 1){
                return i;
            }
        }
        return -1;
    }
};
```

### 面试题50-2-字符流中第一个不重复的字符

#### 题目描述

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。  
输出描述:  
如果当前字符流没有存在出现一次的字符，返回#字符。  

#### 分析

思想和上一题大致一样，关键是要用一个数组来动态记录每个字符的出现次数，每次从哈希表中找符合要求的字符

#### 代码
```c
class Solution
{
public:
    Solution(){
        for(int i = 0; i < 256; ++i){
            occurrence[i] = -1;
        }
        index = 0;
    }
  //Insert one char from stringstream
    void Insert(char ch)
    {
        if(occurrence[(int)ch] == -1){
            occurrence[(int)ch] = index;
        }else{
            occurrence[(int)ch] = -2;
        }
        index++;
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
        char ch = '#';
        int minIndex = numeric_limits<int>::max();
    	for(int i = 0; i < 256; ++i){
            if(occurrence[i]>=0 && occurrence[i] < minIndex){
                ch = (char)i;
                minIndex = occurrence[i];
            }
        }
        return ch;
    }
private:
    int occurrence[256];
    int index;
};
```

### 面试题51-数组中的逆序对

#### 题目描述

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007 

输入描述:

题目保证输入的数组中没有的相同的数字

数据范围：

	对于%50的数据,size<=10^4
	对于%75的数据,size<=10^5
	对于%100的数据,size<=2*10^5

输入例子:

	1,2,3,4,5,6,7,0

输出例子:

	7

#### 分析

这道题利用归并排序的思想，额外开辟了一个数组空间，时间复杂度为O(nlogn)，牛客在书上的基础上额外加上了一个对10000000007取模的条件，注意要放在循环里，为了避免每次都取模，可以先做一个比较判断，当count大于1000000007时再进行取模操作

#### 代码
```c++
class Solution {
public:
    int InversePairs(vector<int> data) {
        if(data.size() == 0){
            return 0;
        }
        vector<int> copy = data;
        int count = InversePairsCore(data, copy, 0, data.size()-1);
        return count;
    }
    int InversePairsCore(vector<int> &data, vector<int> &copy, int start, int end){
        if(start == end){
            copy[start] = data[start];
            return 0;
        }
        int length = (end - start)/2;
        int left = InversePairsCore(copy, data, start, start + length);
        int right = InversePairsCore(copy, data, start + length + 1, end);
        int i = start + length;
        int j = end;
        int indexCopy = end;
        int count = 0;
        while(i >= start && j >= start + length + 1){
            if(data[i] > data[j]){
                copy[indexCopy] = data[i];
                --indexCopy;
                --i;
                count += (j - start - length);
                if(count>=1000000007)//数值过大求余
                {
                    count%=1000000007;
                }
            }else{
                copy[indexCopy] = data[j];
                --indexCopy;
                --j;
            }
        }
        while(i >= start){
            copy[indexCopy] = data[i];
            --indexCopy;
            --i;
        }
        while(j >= start + length + 1){
            copy[indexCopy] = data[j];
            --indexCopy;
            --j;
        }
        return (left + right + count)%1000000007;
    }
};
```

### 面试题52-两个链表的第一个公共结点

#### 题目描述

输入两个链表，找出它们的第一个公共结点。

#### 分析

两种思路：
- 利用两个栈，实现对两个链表进行从后往前的比较，注意判断有链表为空的情况
- 先遍历一次，获得两个链表的长度，让长的链表先走一段，然后再两个链表一起走，直到遇到相同的节点停止，这种思路在时间和空间上都优于第一种

#### 代码
- 思路一
```c++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        if (pHead1 == NULL || pHead2 == NULL) {
			return NULL;
		}
        stack<ListNode*> s1;
        stack<ListNode*> s2;
        ListNode* temp1 = pHead1;
        while(temp1 != NULL){
            s1.push(temp1);
            temp1 = temp1->next;
        }
        ListNode* temp2 = pHead2;
        while(temp2 != NULL){
            s2.push(temp2);
            temp2 = temp2->next;
        }
        ListNode* pCommonNode = s1.top();
        if(pCommonNode != s2.top()){
            return NULL;
        }
        while(!s1.empty() && !s2.empty()){
            if(s1.top() == s2.top()){
                pCommonNode = s1.top();
                s1.pop();
                s2.pop();
            }else{
                break;
            }
        }
        return pCommonNode;
    }
};
```
- 思路二
```c++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        unsigned int nLength1 = GetListLength(pHead1);
        unsigned int nLength2 = GetListLength(pHead2);
        int nLengthDif = nLength1 - nLength2;
        ListNode* pListHeadLong = pHead1;
        ListNode* pListHeadShort = pHead2;
        if(nLength1 < nLength2){
            pListHeadLong = pHead2;
            pListHeadShort = pHead1;
            nLengthDif = nLength2 - nLength1;
        }
        for(int i = 0; i < nLengthDif; ++i){
            pListHeadLong = pListHeadLong->next;
        }
        for(int i = 0; i < nLength2; ++i){
            if(pListHeadLong == pListHeadShort){
                break;
            }else{
                pListHeadLong = pListHeadLong->next;
                pListHeadShort = pListHeadShort->next;
            }
        }
        ListNode* pCommonNode = pListHeadLong;
        return pCommonNode;
    }
    unsigned int GetListLength(ListNode* pHead){
        unsigned int nLength = 0;
        ListNode* pNode = pHead;
        while(pNode != NULL){
            ++nLength;
            pNode = pNode->next;
        }
        return nLength;
    }
};
```
- 牛客大神版：
```c++
//原理其他方法一样，写法比较简单，同时代码有解释。
class Solution {
public:
	ListNode* FindFirstCommonNode(ListNode* pHead1, ListNode* pHead2) {
		/*
		假定 List1长度: a+n  List2 长度:b+n, 且 a<b
		那么 p1 会先到链表尾部, 这时p2 走到 a+n位置,将p1换成List2头部
		接着p2 再走b+n-(n+a) =b-a 步到链表尾部,这时p1也走到List2的b-a位置，还差a步就到可能的第一个公共节点。
		将p2 换成 List1头部，p2走a步也到可能的第一个公共节点。如果恰好p1==p2,那么p1就是第一个公共节点。  或者p1和p2一起走n步到达列表尾部，二者没有公共节点，退出循环。 同理a>=b.
		时间复杂度O(n+a+b)
		*/
		ListNode* p1 = pHead1;
		ListNode* p2 = pHead2;
		while (p1 != p2) {
			if (p1 != NULL) p1 = p1->next;
			if (p2 != NULL) p2 = p2->next;
			if (p1 != p2) {
				if (p1 == NULL) p1 = pHead2;
				if (p2 == NULL) p2 = pHead1;
			}
		}
		return p1;
	}
};
```