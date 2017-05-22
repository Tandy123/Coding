## 【牛客】剑指Offer面试题-数组

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