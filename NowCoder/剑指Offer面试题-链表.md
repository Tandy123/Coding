## 【牛客】剑指Offer面试题-链表

### 面试题6-从尾到头打印链表

#### 题目描述

输入一个链表，从尾到头打印链表每个节点的值。

#### 代码

解法一：用栈实现
```c
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector <int> res;
        stack<ListNode *> nodes;
        
        ListNode *pNode = head;
        while(pNode != NULL)
        {
            nodes.push(pNode);
            pNode = pNode->next;
        }
        while(!nodes.empty())
        {
            pNode = nodes.top();
            res.push_back(pNode->val);
            nodes.pop();
        }
        return res;
    }
};
```

解法二：用递归实现
```c
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> res;
    vector<int> &printListFromTailToHead(ListNode* head) {
        if(head != nullptr)
        {
            if(head->next != nullptr)
            {
                printListFromTailToHead(head->next);
            }
            res.push_back(head->val);
        }
        return res;
    }
};
```

### 面试题22-链表中倒数第k个节点

#### 题目描述

输入一个链表，输出该链表中倒数第k个结点。

#### 分析

想到双指针并不难，难的是保证代码的鲁棒性，考虑下面三种情况：
- 给定链表为空；
- k=0；
- 链表长度小于k

#### 代码
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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
    	if(pListHead == nullptr || k == 0)
            return nullptr;
        ListNode *pAhead = pListHead;
        for(int i = 0; i < k - 1; i++){
            if(pAhead -> next != NULL){
                pAhead = pAhead->next;
            }else{
                return nullptr;
            }
        }
        ListNode *pBehind = pListHead;
        while(pAhead->next != NULL){
            pAhead = pAhead->next;
            pBehind = pBehind->next;
        }
        return pBehind;
    }
};
```

### 面试题23-链表中环的入口节点

#### 题目描述

一个链表中包含环，请找出该链表的环的入口结点。

#### 分析

书上的思路还是比较直接的：
- 先快慢指针判断是否有环
- 然后找到环的节点数
- 再利用间隔固定的双指针找到入口节点

还有一个稍微复杂点的思路：
- 先用快慢指针找到相遇点
- 然后同速指针从起点和相遇点分别出发，再次相交的地方的就是入口节点
- 数学公式可证明（略）

#### 代码

- 第一种思路
```c++
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
//*************************思路1**************************
class Solution {
public:
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
		ListNode *meetingNode = MeetingNode(pHead);
        if(meetingNode == nullptr){
            return nullptr;
        }
        ListNode *pNode1 = meetingNode->next;
		int loopnum = 1;
        while(pNode1 != meetingNode){
            pNode1 = pNode1->next;
            loopnum++;
        }
        pNode1 = pHead;
       	while(loopnum>0){
            pNode1 = pNode1->next;
            loopnum--;
        }
        ListNode *pNode2 = pHead;
        while(pNode2 != pNode1){
            pNode1 = pNode1->next;
            pNode2 = pNode2->next;
        }
        return pNode1;   
    }
    ListNode* MeetingNode(ListNode *pHead){
        if(pHead == nullptr){
            return nullptr;
        }
        ListNode *pSlow = pHead;
        ListNode *pFast = pHead;
        while(pSlow != NULL && pFast != NULL){
            pSlow = pSlow->next;
            pFast = pFast->next;
            if(pFast != NULL){
                pFast = pFast->next;
            	if(pSlow == pFast){
                	return pSlow;
            	}
            }
        }
        return nullptr;
    }
};
```
- 第二种思路

```c++
//*************************思路2**************************
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
		ListNode *meetingNode = MeetingNode(pHead);
        if(meetingNode == nullptr){
            return nullptr;
        }
        ListNode *pNode1 = meetingNode;
        ListNode *pNode2 = pHead;
        while(pNode1 != pNode2){
            pNode1 = pNode1->next;
            pNode2 = pNode2->next;
        }
        return pNode1;   
    }
    ListNode* MeetingNode(ListNode *pHead){
        if(pHead == nullptr){
            return nullptr;
        }
        ListNode *pSlow = pHead;
        ListNode *pFast = pHead;
        while(pSlow != NULL && pFast != NULL){
            pSlow = pSlow->next;
            pFast = pFast->next;
            if(pFast != NULL){
                pFast = pFast->next;
            	if(pSlow == pFast){
                	return pSlow;
            	}
            }
        }
        return nullptr;
    }
};
```