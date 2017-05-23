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