## 【牛客】剑指Offer面试题-栈和队列

### 面试题9.1 用两个栈实现队列

#### 题目描述

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

#### 分析

通过例子分析，注意队列空的时候弹出要做异常抛出

#### 代码
```c
class Solution
{
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        if(stack2.size() == 0)
        {
            while(stack1.size()>0)
            {
                int data = stack1.top();
                stack1.pop();
                stack2.push(data);
            }
        }
        //if(stack2.size() == 0)
        //{
        //    throw new exception("queue is emtpy");
        //}        
        int head = stack2.top();
        stack2.pop();
        return head;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```

### 面试题9.2 用两个队列实现栈

#### 题目描述

用两个队列实现一个栈。栈的声明如下，请实现它的两个函数appendTail和deleteTail，分别完成在栈尾部插入结点和在栈尾部删除结点的功能。

#### 分析

通过例子分析，注意栈空的时候弹出要做异常抛出

#### 代码

直接写成了一个模板

```c
#include <queue>
#include <exception>

using namespace std;

template <typename T> class CStack
{
public:
	CStack(void);
	~CStack(void);

	// 在队列末尾添加一个结点
	void appendTail(const T& node);

	// 删除队列的头结点
	T deleteTail();

private:
	queue<T> queue1;
	queue<T> queue2;
};

template <typename T> CStack<T>::CStack(void)
{
}

template <typename T> CStack<T>::~CStack(void)
{
}

template<typename T> void CStack<T>::appendTail(const T& element)
{
	if (queue2.size() == 0)
	{
		queue1.push(element);
	}
	else
	{
		queue2.push(element);
	}
}

template<typename T> T CStack<T>::deleteTail()
{
	T tail;
	if (queue2.size() == 0)
	{
		while (queue1.size()>1)
		{
			T& data = queue1.front();
			queue1.pop();
			queue2.push(data);
		}
		if (queue1.size() == 0)
			throw new exception("stack is empty");

		tail = queue1.front();
		queue1.pop();
	}
	else if (queue1.size() == 0)
	{
		while (queue2.size()>1)
		{
			T& data = queue2.front();
			queue2.pop();
			queue1.push(data);
		}
		if (queue2.size() == 0)
			throw new exception("stack is empty");

		tail = queue2.front();
		queue2.pop();
	}
	return tail;
}
```

### 面试题30-包含min函数的栈

#### 题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的min函数。

#### 分析
借助辅助栈，面试的时候最好是可以像书上那样写成一个模板类

#### 代码

- 牛客版：
````
class Solution {
public:
    stack<int> m_data;
    stack<int> m_min;
    void push(int value) {
        m_data.push(value);
        if(m_min.size() == 0 || m_min.top() > value){
            m_min.push(value);
        }
        else {
            m_min.push(m_min.top());
        }
    }
    void pop() {
        //assert(m_data.size()>0 && m_min.size()>0);
        m_data.pop();
        m_min.pop();
    }
    int top() {
       	//assert(m_data.size()>0);
        return m_data.top();
    }
    int min() {
        //assert(m_data.size()>0 && m_min.size()>0);
        return m_min.top();
    }
};
```
- 剑指Offer版：
```c++
#pragma once

#include <stack>
#include <assert.h>

template <typename T> class StackWithMin
{
public:
    StackWithMin() {}
    virtual ~StackWithMin() {}

    T& top();
    const T& top() const;

    void push(const T& value);
    void pop();

    const T& min() const;

    bool empty() const;
    size_t size() const;

private:
    std::stack<T>   m_data;     // 数据栈，存放栈的所有元素
    std::stack<T>   m_min;      // 辅助栈，存放栈的最小元素
};

template <typename T> void StackWithMin<T>::push(const T& value)
{
    // 把新元素添加到辅助栈
    m_data.push(value);

    // 当新元素比之前的最小元素小时，把新元素插入辅助栈里；
    // 否则把之前的最小元素重复插入辅助栈里
    if(m_min.size() == 0 || value < m_min.top())
        m_min.push(value);
    else
        m_min.push(m_min.top());
}

template <typename T> void StackWithMin<T>::pop()
{
    assert(m_data.size() > 0 && m_min.size() > 0);

    m_data.pop();
    m_min.pop();
}


template <typename T> const T& StackWithMin<T>::min() const
{
    assert(m_data.size() > 0 && m_min.size() > 0);

    return m_min.top();
}

template <typename T> T& StackWithMin<T>::top()
{
    return m_data.top();
}

template <typename T> const T& StackWithMin<T>::top() const
{
    return m_data.top();
}

template <typename T> bool StackWithMin<T>::empty() const
{
    return m_data.empty();
}

template <typename T> size_t StackWithMin<T>::size() const
{
    return m_data.size();
}
```

### 面试题31-栈的压入、弹出序列

#### 题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4，5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

#### 分析
借助辅助栈，面试的时候最好是可以像书上那样写成一个模板类

#### 代码

- 剑指Offer版：
````
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        bool bPossible = false;
        if(!pushV.empty() && !popV.empty()){
            stack<int> stackData;
            int nextPush = 0;
            int nextPop = 0;        
            while(nextPop < popV.size()){
  // 当辅助栈的栈顶元素不是要弹出的元素
            	// 先压入一些数字入栈
                while(stackData.empty() || stackData.top() != popV[nextPop]){
// 如果所有数字都压入辅助栈了，退出循环                    
·		if(nextPush == pushV.size()){
                        break;
                    }
                    stackData.push(pushV[nextPush]);
                    nextPush++;
            	}
                if(stackData.top() != popV[nextPop]){
                    break;
                }
                stackData.pop();
                nextPop++;
            }
            if(nextPush == pushV.size()&& nextPop == popV.size() && stackData.size() == 0){
            	bPossible = true;
            }
        }
        return bPossible;
    }
};
```
- 牛客版：
```c++
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        if(pushV.size() == 0) return false;
        vector<int> stack;
        for(int i = 0,j = 0 ;i < pushV.size();){
            stack.push_back(pushV[i++]);
            while(j < popV.size() && stack.back() == popV[j]){
                stack.pop_back();
                j++;
            }       
        }
        return stack.empty();
    }
};
```