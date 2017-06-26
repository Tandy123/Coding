## 【牛客】剑指Offer面试题64

### 面试题64-求1+2+…+n

#### 题目描述
求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

#### 分析

书上提供了很多种方法，但是感觉都比较麻烦，牛客上的方法更为巧妙

#### 代码
- 剑指Offer1（利用构造函数求解）：
```c++
class Temp{
public:
    Temp(){
        ++N;
        Sum += N;
    }
    static void Reset(){
        N = 0;
        Sum = 0;
    }
    static unsigned int GetSum(){
        return Sum;
    }
private:
    static unsigned int N;
    static unsigned int Sum;
};
unsigned int Temp::N = 0;
unsigned int Temp::Sum = 0;
class Solution {
public:
    int Sum_Solution(int n) {
        Temp::Reset();
        Temp *a = new Temp[n];
        delete []a;
        a = nullptr;
        return Temp::GetSum();
    }
};
```
- 剑指Offer2（利用虚函数求解）：
```c++
class A;
A* Array[2];
class A{
public:
    virtual unsigned int Sum(unsigned int n){
        return 0;
    }
};
class B:public A{
public:
    virtual unsigned int Sum(unsigned int n){
        return Array[!!n]->Sum(n-1) + n;
    }
};
class Solution {
public:
    int Sum_Solution(int n) {
        A a;
        B b;
        Array[0] = &a;
        Array[1] = &b;
        int value = Array[1]->Sum(n);
        return value;
    }
};
```
- 剑指Offer3（利用函数指针求解）：
```c++
typedef unsigned int (*fun)(unsigned int);
unsigned int A(unsigned int n){
    return 0;
}
unsigned int B(unsigned int n){
    static fun f[2] = {A,B};
    return n + f[!!n](n - 1);
}
class Solution {
public:
    int Sum_Solution(int n) {
        int value = B(n);
        return value;
    }
};
```
- 牛客大神1（利用逻辑短路）：
```c++
class Solution {
public:
    int Sum_Solution(int n) {
        int ans = n;
        ans && (ans += Sum_Solution(n - 1));
        return ans;
    }
};
```
- 牛客大神2（利用sizeof加位运算模拟除二）
```c++
class Solution {
public:
    int Sum_Solution(int n) {
        bool a[n][n+1];
        return sizeof(a)>>1;
    }
};
```