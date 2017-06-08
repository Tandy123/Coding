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

### 面试题24-反转链表

#### 题目描述

输入一个链表，反转链表后，输出链表的所有元素。

#### 分析

用三个指针，注意结束条件

测试时考虑如下输入：输入链表头指针为nullptr或者链表中只有一个节点

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
};*/
class Solution {//4min
public:
    ListNode* ReverseList(ListNode* pHead) {
		ListNode* pReverseHead = nullptr;
        ListNode* pNode = pHead;
        ListNode* pPre =nullptr;
        while(pNode != nullptr){
            ListNode* pNext = pNode->next;
            if(pNext == NULL){
                pReverseHead = pNode;
            }
           	pNode->next = pPre;
            pPre = pNode;
            pNode = pNext;
        }
        return pReverseHead;
    }
};
```


### 面试题25-合并两个排序的链表

#### 题目描述

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

#### 分析

有两种思路：
- 递归：注意结束条件
- 非递归：注意处理新的头结点

#### 代码

- 递归版本：
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
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1 == NULL){
            return pHead2;
        }
        if(pHead2 == NULL){
            return pHead1;
        }
        ListNode* pNode = nullptr;
        if(pHead1->val < pHead2->val){
            pNode = pHead1;
        	pNode->next = Merge(pHead1->next, pHead2);
        }else{
            pNode = pHead2;
            pNode->next = Merge(pHead1, pHead2->next);
        }
        return pNode;
    }
};
```

- 非递归版：
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
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1 == NULL){
            return pHead2;
        }
        if(pHead2 == NULL){
            return pHead1;
        }
        ListNode* pMergeHead = NULL;
        ListNode* pNode = NULL;
        while(pHead1 != NULL && pHead2 != NULL){
            if(pHead1->val < pHead2->val){
                if(pMergeHead == NULL){
                    pMergeHead = pHead1;
                    pNode = pMergeHead;
                }else{
                    pNode->next = pHead1;
                    pNode = pNode->next;
                }
                pHead1 = pHead1->next;//这句没写会死循环
            }else{
                if(pMergeHead == NULL){
                    pMergeHead = pHead2;
                    pNode = pMergeHead;
                }else{
                    pNode->next = pHead2;
                    pNode = pNode->next;
                }
                pHead2 = pHead2->next;//这句没写会死循环
            }
        }
        if(pHead1 == NULL){
            pNode->next = pHead2;
        }
        if(pHead2 == NULL){
            pNode->next = pHead1;
        }
        return pMergeHead;
    }
};
```

### 面试题35-复杂链表的复制

#### 题目描述

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

#### 分析

分治的思想，把问题分解成三个小问题，逐一攻破，链表操作，注意细节处理，边界终止条件，最好画图分析。

#### 代码

- 剑指Offer版：
```c++
/*
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        CloneNode(pHead);
        ConnectRandom(pHead);
        return ReconnectNodes(pHead);
    }
    void CloneNode(RandomListNode* pHead){
        RandomListNode* pNode = pHead;
        while(pNode != NULL){
            RandomListNode* pClone = new RandomListNode(0);
            pClone->label = pNode->label;
            pClone->next = pNode->next;
            pNode->next = pClone;
            pNode = pClone->next;
        }
    }
    void ConnectRandom(RandomListNode* pHead){
        RandomListNode* pNode = pHead;
        while(pNode != NULL){
            RandomListNode* pClone = pNode->next;
            if(pNode->random != NULL){
                pClone->random = pNode->random->next;
            }            
            pNode = pClone->next;
        }
    }
    RandomListNode* ReconnectNodes(RandomListNode* pHead){
        RandomListNode* pNode = pHead;
        RandomListNode* pCloneHead = nullptr;
        RandomListNode* pPreClone = nullptr;
        if(pNode != NULL){
            pCloneHead = pPreClone = pNode->next;
            pNode->next = pPreClone->next;
            pNode = pNode->next;
        }
        while(pNode != NULL){
            pPreClone->next = pNode->next;
            pPreClone = pPreClone->next;
            pNode->next = pPreClone->next;
            pNode = pNode->next;
        }
        return pCloneHead;
    }
};
```

- 牛客大神版，拆分的思想值得借鉴：
```c++
class Solution {
public:
    /*
        1、复制每个节点，如：复制节点A得到A1，将A1插入节点A后面
        2、遍历链表，A1->random = A->random->next;
        3、将链表拆分成原链表和复制后的链表
    */
     
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if(!pHead) return NULL;
        RandomListNode *currNode = pHead;
        while(currNode){
            RandomListNode *node = new RandomListNode(currNode->label);
            node->next = currNode->next;
            currNode->next = node;
            currNode = node->next;
        }
        currNode = pHead;
        while(currNode){
            RandomListNode *node = currNode->next;
            if(currNode->random){                
                node->random = currNode->random->next;
            }
            currNode = node->next;
        }
        //拆分
        RandomListNode *pCloneHead = pHead->next;
        RandomListNode *tmp;
        currNode = pHead;
        while(currNode->next){
            tmp = currNode->next;
            currNode->next =tmp->next;
            currNode = tmp;
        }
        return pCloneHead;
    }
};
```