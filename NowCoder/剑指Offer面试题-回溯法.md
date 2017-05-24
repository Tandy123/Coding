## 【牛客】剑指Offer面试题-回溯法

### 面试题12-矩阵中的路径

#### 题目描述

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。 例如 a b c e s f c s a d e e 矩阵中包含一条字符串"bccced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。

#### 分析

用回溯来实现

#### 代码
```c
class Solution {
public:
    bool hasPath(char* matrix, int rows, int cols, char* str)
    {
        if(matrix == nullptr || rows < 1 || cols < 1 || str == nullptr)
            return false;
    	bool *visited = new bool[rows * cols];
        memset(visited, 0, rows* cols);
        
        int pathLength = 0;
        for(int row = 0; row < rows; ++row)
        {
            for(int col = 0; col < cols; ++col)
            {
                if(hasPathCore(matrix, rows, cols, row, col, str, pathLength, visited))
                {
                    return true;
                }
            }
        }
        delete []visited;//不要忘了释放空间
        return false;
    }

    bool hasPathCore(char *matrix, int rows, int cols, int row, int col, char *str, int &pathLength, bool *visited)
    {
        if(str[pathLength] == '\0')
        {
            return true;
        }
        
        bool hasPath = false;
        if(row >= 0 && row < rows && col >= 0 
           && col < cols && visited[row * cols + col] == false 
           && matrix[row * cols + col] == str[pathLength])
        {
            ++pathLength;
            visited[row * cols + col] = true;
            hasPath = hasPathCore(matrix, rows, cols, row + 1, col, str, pathLength, visited)
                ||hasPathCore(matrix, rows, cols, row, col + 1, str, pathLength, visited)
                ||hasPathCore(matrix, rows, cols, row - 1, col, str, pathLength, visited)
                ||hasPathCore(matrix, rows, cols, row, col - 1, str, pathLength, visited);
            if(!hasPath){//这句判断写在这里，才能体现回溯，回到上一步的状态
            	--pathLength;
            	visited[row * cols + col] = false;
        	}
        }
        
        return hasPath;
    }
};
```

## 【牛客】剑指Offer面试题-回溯法

### 面试题13-机器人的运动范围

#### 题目描述

地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

#### 分析

这题试求解空间的大小，所以和上一题求最优解不同，没有一个需要进行复原的变量，如上一题的pathLength，这里要遍历所有的可行解，剑指Offer里的思路走的方向有点多了，因为是从(0,0)开始走的，所以只要考虑往右和往下的情况就行了。

#### 代码
```c
class Solution {
public:
    int movingCount(int threshold, int rows, int cols)
    {
        if(threshold < 0 || rows <= 0 || cols <= 0)
        {
            return 0;
        }
        bool *visited = new bool[rows * cols];
        memset(visited, false, rows* cols);
        int count = movingCountCore(threshold, rows, cols, 0, 0, visited);
        delete []visited;
        return count;
    }
    int movingCountCore(int threshold, int rows, int cols, int row, int col, bool *visited)
    {
        int count = 0;
        if(check(threshold, rows, cols, row, col, visited))
        {
            visited[row * cols + col] = true;
            count = 1 + movingCountCore(threshold, rows, cols, row + 1, col, visited)
                + movingCountCore(threshold, rows, cols, row, col + 1, visited);
               	//+ movingCountCore(threshold, rows, cols, row - 1, col, visited)
                //+ movingCountCore(threshold, rows, cols, row, col - 1, visited);
        }
        return count;
    }
    bool check(int threshold, int rows, int cols, int row, int col, bool *visited)
    {
        if(row >= 0 && row < rows 
           && col >= 0 && col < cols 
           && visited[row * cols + col]== false
           && getDigitSum(row) + getDigitSum(col)<= threshold)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
    int getDigitSum(int num)
    {
        int sum = 0;
        while(num > 0)
        {
            sum += num % 10;
            num /= 10;
        }
        return sum;
    }
};
```
