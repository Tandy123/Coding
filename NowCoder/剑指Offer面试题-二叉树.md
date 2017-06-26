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

### 面试题27-二叉树的镜像

#### 题目描述
操作给定的二叉树，将其变换为源二叉树的镜像。

#### 输入描述:
二叉树的镜像定义：源二叉树 

    	    8
    	   /  \
    	  6   10
    	 / \  / \
    	5  7 9 11
    	镜像二叉树
    	    8
    	   /  \
    	  10   6
    	 / \  / \
    	11 9 7  5 

#### 分析

递归实现

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
    void Mirror(TreeNode *pRoot) {
        if(pRoot == NULL){
            return;
        }
        if(pRoot->left == NULL && pRoot->right == NULL){
            return;
        }
        
		TreeNode *pTemp = pRoot->left;
        pRoot->left = pRoot->right;
        pRoot->right = pTemp;
        
        if(pRoot->left){
            Mirror(pRoot->left);
        }
        if(pRoot->right){
            Mirror(pRoot->right);
        }
    }
};
```

### 面试题28-对称的二叉树

#### 题目描述
请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

#### 分析
递归实现，前序遍历二叉树，比较中左右和中右左的序列，考虑空节点

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
};
*/
class Solution {
public:
    bool isSymmetrical(TreeNode* pRoot)
    {
    	return isSymmetrical(pRoot, pRoot);
    }
	bool isSymmetrical(TreeNode* pRoot1, TreeNode* pRoot2){
        if(pRoot1 == NULL && pRoot2==NULL){
            return true;
        }
        if(pRoot1 == NULL || pRoot2 == NULL){
            return false;
        }
        if(pRoot1->val != pRoot2->val){
            return false;
        }
        return isSymmetrical(pRoot1->left, pRoot2->right) && isSymmetrical(pRoot1->right, pRoot2->left);
    }
};
```

### 面试题32-1-不分行从上到下打印二叉树

#### 题目描述
从上往下打印出二叉树的每个节点，同层节点从左至右打印。

#### 分析

按层遍历，用队列实现，书上用的是双向队列deque，感觉并没有什么必要

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
    vector<int> PrintFromTopToBottom(TreeNode* root) {
		vector<int> res;
        if(root == NULL){
            return res;
        }
        queue<TreeNode* > qData;
        qData.push(root);
        while(!qData.empty()){
            TreeNode* pTemp = qData.front();
            if(pTemp->left != NULL){
                qData.push(pTemp->left);
            }
            if(pTemp->right != NULL){
                qData.push(pTemp->right);
            }
            res.push_back(pTemp->val);
            qData.pop();
        }
        return res;
    }
};
```

### 面试题32-2-分行从上到下打印二叉树

#### 题目描述
从上到下按层打印二叉树，同一层的结点按从左到右的顺序打印，每一层打印到一行。

#### 分析

按层遍历，用队列实现，书上用的是双向队列deque，感觉并没有什么必要

#### 代码

```c
struct BinaryTreeNode 
{
    int                    m_nValue; 
    BinaryTreeNode*        m_pLeft;  
    BinaryTreeNode*        m_pRight; 
};
void Print(BinaryTreeNode* pRoot)
{
	if (pRoot == nullptr)
	{
		return;
	}
	std::deque<BinaryTreeNode*> nodes;
	nodes.push_back(pRoot);
	int toBePrinted = 1;
	int nextLevel = 0;
	while (!nodes.empty()) {
		BinaryTreeNode* pNode = nodes.front();
		printf("%d ", pNode->m_nValue);
		if (pNode->m_pLeft)
		{
			nodes.push_back(pNode->m_pLeft);
			nextLevel++;
		}
		if (pNode->m_pRight)
		{
			nodes.push_back(pNode->m_pRight);
			nextLevel++;
		}
		nodes.pop_front();
		toBePrinted--;
		if (toBePrinted == 0) {
			toBePrinted = nextLevel;
			nextLevel = 0;
			printf("\n");
		}
	}
}
```

### 面试题32-2-分行从上到下打印二叉树

#### 题目描述
从上到下按层打印二叉树，同一层的结点按从左到右的顺序打印，每一层打印到一行。

#### 分析
用队列进行层序遍历，同时用两个变量记录每层的数量，用于输出换行符

#### 代码

```c
struct BinaryTreeNode 
{
    int                    m_nValue; 
    BinaryTreeNode*        m_pLeft;  
    BinaryTreeNode*        m_pRight; 
};
void Print(BinaryTreeNode* pRoot)
{
	if (pRoot == nullptr)
	{
		return;
	}
	std::deque<BinaryTreeNode*> nodes;
	nodes.push_back(pRoot);
	int toBePrinted = 1;
	int nextLevel = 0;
	while (!nodes.empty()) {
		BinaryTreeNode* pNode = nodes.front();
		printf("%d ", pNode->m_nValue);
		if (pNode->m_pLeft)
		{
			nodes.push_back(pNode->m_pLeft);
			nextLevel++;
		}
		if (pNode->m_pRight)
		{
			nodes.push_back(pNode->m_pRight);
			nextLevel++;
		}
		nodes.pop_front();
		toBePrinted--;
		if (toBePrinted == 0) {
			toBePrinted = nextLevel;
			nextLevel = 0;
			printf("\n");
		}
	}
}
```

### 面试题32-3-按之字形顺序打印二叉树

#### 题目描述
（注：这道题在试题广场里可以搜到）  
从上到下按层打印二叉树，同一层的结点按从左到右的顺序打印，每一层打印到一行。

#### 分析
用队列进行层序遍历，同时用两个变量记录每层的数量，用于输出换行符

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
};
*/
class Solution {
public:
    vector<vector<int> > Print(TreeNode* pRoot) {
        vector<vector<int>> res;
        if(pRoot == NULL){
            return res;
        }
        stack<TreeNode*> level[2];
        int current = 0;
        int next = 1;
        level[current].push(pRoot);
        vector<int> temp;
        while(!level[0].empty() || !level[1].empty()){
            TreeNode* pTreeNode = level[current].top();
            temp.push_back(pTreeNode->val);
            level[current].pop();
            if(current == 0){
                if(pTreeNode->left != NULL){
                    level[next].push(pTreeNode->left);
                }
                if(pTreeNode->right != NULL){
                    level[next].push(pTreeNode->right);
                }
            }else{
                
                if(pTreeNode->right != NULL){
                    level[next].push(pTreeNode->right);
                }
                if(pTreeNode->left != NULL){
                    level[next].push(pTreeNode->left);
                }
            }
            if(level[current].empty()){
                res.push_back(temp);
                temp.clear();
                current = 1-current;
                next = 1 - next;
            }
        }
        return res;
    }
};
```

### 面试题33-二叉搜索树的后序遍历序列

#### 题目描述
输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

#### 分析
递归实现

#### 代码
- 剑指Offer版：
```c
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
		if(sequence.size() == 0){
            return false;
        }
        return VerifySquenceOfBSTRecursive(sequence, 0, sequence.size()-1);
    }
    bool VerifySquenceOfBSTRecursive(vector<int> &sequence, int start, int end){
        int root = sequence[end];
        int i = start;
        for(; i < end; ++i){
            if(sequence[i] >= root){
                break;
            }
        }
        int j = i;
        for(;j < end; ++j){
            if(sequence[j]<root){
                return false;
            }
        }
        bool left = true;
        if(i>start){
            left = VerifySquenceOfBSTRecursive(sequence, start, i-1);
        }
        bool right = true;
        if(i<end){
            right = VerifySquenceOfBSTRecursive(sequence, i, end-1);
        }
        return left && right;
    }
};
```
- 牛客大神版：
```c++
class Solution {
    bool judge(vector<int>& a, int l, int r){
        if(l >= r) return true;
        int i = r;
        while(i > l && a[i - 1] > a[r]) --i;
        for(int j = i - 1; j >= l; --j) if(a[j] > a[r]) return false;
        return judge(a, l, i - 1) && (judge(a, i, r - 1));
    }
public:
    bool VerifySquenceOfBST(vector<int> a) {
        if(!a.size()) return false;
        return judge(a, 0, a.size() - 1);
    }
};
```
- 牛客大神非递归：
```c++
class Solution {
public:
bool VerifySquenceOfBST(vector<int> sequence) {
        int size = sequence.size();
        if(0==size)return false;
 
        int i = 0;
        while(--size)
        {
            while(sequence[i++]<sequence[size]);
            while(sequence[i++]>sequence[size]);
 
            if(i<size)return false;
            i=0;
        }
        return true;
    }
};
```

### 面试题34-二叉树中和为某一值的路径

#### 题目描述
输入一颗二叉树和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。

#### 分析
思路不难，就是写的时候要细心一点

#### 代码
- 剑指Offer版：
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
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        vector<vector<int>> res;
        if(root == nullptr){
            return res;
        }
		vector<TreeNode*> path;
        FindPathRecursive(root, path, expectNumber, 0, res);
        return res;
    }
    void FindPathRecursive(TreeNode* root, vector<TreeNode*> &path, int expectNumber, int currentNumber, vector<vector<int>> &res){
        currentNumber += root->val;
        path.push_back(root);
        bool isLeaf = (root->left == NULL)&&(root->right == NULL);
        if(currentNumber== expectNumber && isLeaf){
            vector<TreeNode*>::iterator iter = path.begin();
            vector<int> temp;
           	for(; iter != path.end(); ++iter){
                temp.push_back((*iter)->val);
            }
            res.push_back(temp);
        }
        if(root->left != NULL){
            FindPathRecursive(root->left, path, expectNumber, currentNumber, res);
        }
        if(root->right != NULL){
            FindPathRecursive(root->right, path, expectNumber, currentNumber, res);
        }
        path.pop_back();
    }
};
```

### 面试题36-二叉搜索树与双向链表

#### 题目描述
输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

#### 分析
中序遍历，递归实现，注意每次递归返回时要记录当前链表的最后一个节点

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
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
     	TreeNode* pLastNodeInList = nullptr;
        ConvertNode(pRootOfTree, &pLastNodeInList);
        TreeNode* pHeadOfList = pLastNodeInList;
        while(pHeadOfList != nullptr && pHeadOfList->left != NULL){
            pHeadOfList = pHeadOfList->left;
        }
        return pHeadOfList;
    }
    void ConvertNode(TreeNode* pNode, TreeNode** pLastNodeInList){
        if(pNode == NULL){
            return;
        }
        if(pNode->left != NULL){
            ConvertNode(pNode->left, pLastNodeInList);
        }
        pNode->left = *pLastNodeInList;
        if(*pLastNodeInList != nullptr){
            (*pLastNodeInList)->right = pNode;
        }
        *pLastNodeInList = pNode;
        if(pNode->right != NULL){
            ConvertNode(pNode->right, pLastNodeInList);
        }
    }
};
```

### 面试题37-序列化二叉树

#### 题目描述
请实现两个函数，分别用来序列化和反序列化二叉树

#### 分析
题目的要求有点麻烦，要返回char*  
只能先用string来做，最后转化为char\*，需要注意的地方，反序列化的时候，要传char\**，来记录指针的移动，因为要区分每个数字序列化的时候要用‘,’来对每个数字进行分割，反序列化的时候要注意把字符转成数字，这题真心麻烦

#### 代码
```c
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/
class Solution {
public:
    char* Serialize(TreeNode *root) {    
        if(root == NULL){
            return NULL;
        }
        string str;
        Serialize(root, str);
        char* cstr = new char[str.length() + 1];
        int i;
        for(i = 0; i < str.length()+1; ++i){
            cstr[i] = str[i];
        }
        cstr[i] = '\0';
        return cstr;
    }
    void Serialize(TreeNode* root, string &str){
        if(root == NULL){
            str += '$';
            return;
        }
        string val = to_string(root->val);
        str+=val;
        str += ',';
        Serialize(root->left, str);
        Serialize(root->right, str);
    }
    TreeNode* Deserialize(char *str) {
    	if(str == nullptr){
            return nullptr;
        }
        return Deserialize2(&str);
    }
    TreeNode* Deserialize2(char **str) {
        if(**str == '$'){
            ++(*str);
            return nullptr;
        }
		int num = 0;
        while(**str != '\0' && **str != ','){
            num = num*10 + ((**str) - '0');
            ++(*str);
        }
        TreeNode* pNode = new TreeNode(num);
        if(**str == '\0')
            return pNode;
        else
            (*str)++;
        pNode->left = Deserialize2(str);
        pNode->right = Deserialize2(str);
        return pNode;
    }
};
```

### 面试题54-二叉搜索树的第k大节点

#### 题目描述
给定一颗二叉搜索树，请找出其中的第k大的结点。例如， 5 / \ 3 7 /\ /\ 2 4 6 8 中，按结点数值大小顺序第三个结点的值为4。

#### 分析
中序遍历，注意边界条件，注意返回节点之后不再进行更多的遍历

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
};
*/
class Solution {
public:
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        if(pRoot == nullptr || k <= 0){
            return nullptr;
        }
        return KthNodeCore(pRoot, k);
    }
    TreeNode* KthNodeCore(TreeNode* pRoot, int &k){
        TreeNode* node = nullptr;
        if(pRoot->left != nullptr){
            node = KthNodeCore(pRoot->left, k);
        }
        if(node == nullptr){
            if(k == 1){
                node = pRoot;
            }else{
                k--;
            }
        }
        if(node == nullptr && pRoot->right != nullptr){
            node = KthNodeCore(pRoot->right, k);
        }
        return node;
    }
};
```

### 面试题55-1-二叉树的深度

#### 题目描述
输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

#### 分析
递归实现，某个node的深度=max（node->left的深度，node->right的深度）+1

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
    int TreeDepth(TreeNode* pRoot)
    {
    	if(pRoot == NULL){
            return 0;
        }
        int left = TreeDepth(pRoot->left);
        int right = TreeDepth(pRoot->right);
        return left > right ? left + 1:right + 1;
    }
};
```

### 面试题55-2-平衡二叉树

#### 题目描述
输入一棵二叉树，判断该二叉树是否是平衡二叉树。

#### 分析
后序遍历，返回是否平衡以及当前的深度，当找到一个不平衡的子树时，其他的部分也就没有必要继续遍历了

#### 代码
```c
class Solution {
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
		int depth = 0;
        return IsBalanced(pRoot, depth);
    }
    bool IsBalanced(const TreeNode* pRoot, int &depth){
        if(pRoot == nullptr){
            depth = 0;
            return true;
        }
        int left, right;
        if(IsBalanced(pRoot->left,left) && IsBalanced(pRoot->right,right)){
            if(abs(left - right) <= 1){
                depth = (left > right?left:right) + 1;
                return true;
            }
        }
        return false;
    }
};
```