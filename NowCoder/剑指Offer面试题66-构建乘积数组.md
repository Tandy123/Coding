## 【牛客】剑指Offer面试题66

### 面试题66-构建乘积数组

#### 题目描述
给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]\*A[1]\*...\*A[i-1]\*A[i+1]\*...\*A[n-1]。不能使用除法。

#### 分析

将数组B看成是一个矩阵，找规律，通过上下三角连乘求解，时间复杂度O(n)

![](https://uploadfiles.nowcoder.com/images/20160829/841505_1472459965615_8640A8F86FB2AB3117629E2456D8C652)

#### 代码
- 剑指Offer版
```c++
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
    	vector<int> B(A.size(), 0);
        if(A.size() <= 1){
            return B;
        }
        B[0] = 1;
        for(int i = 1; i< A.size(); ++i){
            B[i] = B[i - 1] * A[i - 1];
        }
        int temp = 1;
        for(int i = A.size() - 2; i >= 0; --i){
            temp *= A[i + 1];
            B[i] *= temp;
        }
        return B;
    }
};

```
