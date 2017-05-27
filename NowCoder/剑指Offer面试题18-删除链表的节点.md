## 【牛客】剑指Offer面试题-高质量的代码

### 面试题18-1-删除链表的节点

#### 题目描述

给定单向链表的头指针和一个结点指针，定义一个函数在O(1)时间删除该 结点。

#### 分析

分三种情况考虑：

- 链表中有多个结点，要删除的结点不是尾结点
- 链表只有一个结点，删除头结点（也是尾结点）
- 链表中有多个结点，删除尾结点

#### 代码

```c++
void DeleteNode(ListNode** pListHead, ListNode* pToBeDeleted)
{
    if(!pListHead || !pToBeDeleted)
        return;

    // 链表中有多个结点，要删除的结点不是尾结点
    if(pToBeDeleted->m_pNext != nullptr)
    {
        ListNode* pNext = pToBeDeleted->m_pNext;
        pToBeDeleted->m_nValue = pNext->m_nValue;
        pToBeDeleted->m_pNext = pNext->m_pNext;
 
        delete pNext;
        pNext = nullptr;
    }
    // 链表只有一个结点，删除头结点（也是尾结点）
    else if(*pListHead == pToBeDeleted)
    {
        delete pToBeDeleted;
        pToBeDeleted = nullptr;
        *pListHead = nullptr;
    }
    // 链表中有多个结点，删除尾结点
    else
    {
        ListNode* pNode = *pListHead;
        while(pNode->m_pNext != pToBeDeleted)
        {
            pNode = pNode->m_pNext;            
        }
 
        pNode->m_pNext = nullptr;
        delete pToBeDeleted;
        pToBeDeleted = nullptr;
    }
}
```

#### 补充

以上代码基于一个假设：要删除的节点的确在链表中，我们一般还需要O(n)的时间才能判断链表中是否包含某一个节点。受到O(1)的限制，我们不得不把确保节点在链表中的责任推给了DeleteNode的调用者。


### 面试题18-2-删除链表重复的节点

#### 题目描述

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

[牛客地址](https://www.nowcoder.com/practice/fc533c45b73a41b0b44ccba763f866ef?tpId=13&tqId=11209&tPage=3&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

#### 分析

需要注意的地方：

- 头节点也可能被删除，所以要借助ListNode **pHead来记录头结点的地址
- 确保pPreNode始终和下一个没有重复的节点连接在一起
- 注意到最后一个节点时，应该先判断是否为空，不然是取不到val的值的

#### 代码

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
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead)
    {
		if(pHead == nullptr)
        {
            return NULL;
        }
        ListNode **pHead0 = &pHead;
        ListNode *pNode = pHead;
        ListNode *pPreNode = nullptr;
        while(pNode != nullptr)
        {
            ListNode *pNext = pNode->next;
            bool needDelete = false;
            if(pNext != nullptr && pNode->val == pNext->val)
            {
                needDelete = true;
            }
            
            if(!needDelete)
            {
                pPreNode = pNode;
                pNode = pNext;
            }
            else
            {
                int value = pNode->val;
                ListNode *pToBeDel = pNode;
                while(pToBeDel != nullptr && pToBeDel->val == value)
                {
                    pNext = pToBeDel->next;
                    
                    delete pToBeDel;
                    pToBeDel = nullptr;
                    
                    pToBeDel = pNext;
                }
                if(pPreNode == nullptr)
                {
                    *pHead0 = pNext;
                }
                else
                {
                    pPreNode->next = pNext;
                }
                pNode = pNext;
            }
        }
        return *pHead0;
    }
};
```