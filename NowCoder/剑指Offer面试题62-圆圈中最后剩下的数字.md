## 【牛客】剑指Offer面试题62

### 面试题62-圆圈中最后剩下的数字

#### 题目描述
每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)

#### 分析

两种思路：

- 思路一：直接用环形链表模拟整个过程，可以使用模板库中的std::list
- 思路二：找到规律，根据规律直接找到最后留下来的数字，效率相当之高。  
书上P302分析得很详细，这里简单缕一下思路：  
如果只求最后一个报数胜利者的话，我们可以用数学归纳法解决该问题，为了讨论方便，先把问题稍微改变一下，并不影响原意：  
问题描述：n个人（编号0~(n-1))，从0开始报数，报到(m-1)的退出，剩下的人继续从0开始报数。求胜利者的编号。      
我们知道第一个人(编号一定是m%n-1) 出列之后，剩下的n-1个人组成了一个新的约瑟夫环（以编号为k=m%n的人开始）:      
    k  k+1  k+2  ... n-2, n-1, 0, 1, 2, ... k-2并且从k开始报0。
现在我们把他们的编号做一下转换：
	k     --> 0
	k+1   --> 1
	k+2   --> 2
	...
	...
	k-2   --> n-2
	k-1   --> n-1
变换后就完完全全成为了(n-1)个人报数的子问题，假如我们知道这个子问题的解：  
例如x是最终的胜利者，那么根据上面这个表把这个x变回去不刚好就是n个人情况的解吗？！！变回去的公式很简单，相信大家都可以推出来：x'=(x+k)%n（因为正向的映射是：(x-k-1)%n，所以反向的映射就是(x+k+1)%n）。  
令f[i]表示i个人玩游戏报m退出最后胜利者的编号，最后的结果自然是f[n]。  
递推公式
	f[1]=0;
	f[i]=(f[i-1]+m)%i;  (i>1)


#### 代码
- 思路一
```c++
class Solution {
public:
    int LastRemaining_Solution(int n, int m)
    {
        if(n < 1 || m < 1){
            return -1;
        }
        list<int> numbers;
        for(int i = 0; i < n; ++i){
            numbers.push_back(i);
        }
        auto current = numbers.begin();
        while(numbers.size() > 1){
            for(int i = 1; i < m; ++i){
                current++;
                if(current == numbers.end()){
                    current = numbers.begin();
                }
            }
            auto next = ++current;
            if(next == numbers.end()){
                next = numbers.begin();
            }
            --current;
            numbers.erase(current);
            current = next;
        }
        return *current;
    }
};
```
- 思路二
```c++
class Solution {
public:
    int LastRemaining_Solution(int n, int m)
    {
        if(n < 1 || m < 1){
            return -1;
        }
        int last = 0;
        for(int i = 2; i <= n; ++i){
            last = (last + m)%i;
        }
        return last;
    }
};
```