## 【牛客】剑指Offer面试题-二叉树

### 面试题7-重建二叉树

#### 题目描述
输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

#### 分析

递归实现

#### 代码

```c
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
		if(pre.size() == 0 || vin.size() == 0)
        {
            return NULL;
        }
        return ConstructCore(pre, 0, pre.size()-1, vin, 0, vin.size() - 1);
    }
    
    TreeNode* ConstructCore(const vector<int> &pre, int pre_start, int pre_end, const vector<int> &vin, int vin_start, int vin_end)
    {
        TreeNode *root = new TreeNode(pre[pre_start]);
        if(pre_start == pre_end)
        {
            if(vin_start == vin_end && pre[pre_start] == vin[vin_start])
            {
                return root;
            }
            else
            {
                return NULL;
            }
        }
        int vin_root = vin_start;
        while(vin_root <= vin_end  && vin[vin_root] != pre[pre_start])
        {
            ++vin_root; 
        }
        if(vin_root > vin_end)
        {
            return NULL;
        }
        int left_length = vin_root - vin_start;
        int left_pre_end = pre_start + left_length;
        if(left_length > 0)
        {
            root->left = ConstructCore(pre, pre_start + 1, left_pre_end, vin, vin_start, vin_root - 1);
        }
        if(left_pre_end < pre_end)
        {
            root->right = ConstructCore(pre, left_pre_end + 1, pre_end, vin, vin_root + 1, vin_end);
        }
        return root;
    }
};
```

### 试题8-二叉树的下一个节点

#### 题目描述

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

#### 分析

分三种情况考虑
- 是否有右节点
- 无右节点时，是否是父节点的左节点
- 无右节点时，是否是父节点的右节点

#### 代码

```c
/*
struct TreeLinkNode {
    int val;
    struct TreeLinkNode *left;
    struct TreeLinkNode *right;
    struct TreeLinkNode *next;
    TreeLinkNode(int x) :val(x), left(NULL), right(NULL), next(NULL) {
        
    }
};
*/
class Solution {
public:
    TreeLinkNode* GetNext(TreeLinkNode* pNode)
    {
        if(pNode == NULL)
        {
            return NULL;
        }
        
        TreeLinkNode *pNext = NULL;
        if(pNode->right != NULL)
        {
            TreeLinkNode *pRight = pNode->right;
            while(pRight->left != NULL)
            {
                pRight = pRight->left;
            }
            pNext = pRight;
        }
        else if(pNode->next != NULL)
        {
	    TreeLinkNode *pCurrent = pNode;
	    TreeLinkNode *pParent = pNode->next;
            while(pParent != NULL && pCurrent == pParent->right)
            {
                pCurrent = pParent;
                pParent = pParent->next;
            }
            pNext = pParent;
        }
        return pNext;
    }
};
```

### 面试题26-树的子结构

#### 题目描述
输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

#### 分析

搞清楚边界条件，注意题目中的“我们约定空树不是任意一个树的子结构”

#### 代码

```c
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
		bool res = false;
        if(pRoot1 != NULL && pRoot2 != NULL){
            if(pRoot1->val == pRoot2->val){
                res = DoesTree1HasTree2(pRoot1, pRoot2);
            }
            if(!res){
                res = HasSubtree(pRoot1->left, pRoot2);
            }
            if(!res){
                res = HasSubtree(pRoot1->right, pRoot2);
            }
        }
        return res;
    }
    bool DoesTree1HasTree2(TreeNode* pRoot1, TreeNode* pRoot2){
        if(pRoot2 == NULL){
            return true;
        }
        if(pRoot1 == NULL){
            return false;
        }
        if(pRoot1->val != pRoot2->val){//少了这个判断，就很容易返回true了= =
            return false;
        }
        return DoesTree1HasTree2(pRoot1->left, pRoot2->left) && DoesTree1HasTree2(pRoot1->right, pRoot2->right);
    }
};
```