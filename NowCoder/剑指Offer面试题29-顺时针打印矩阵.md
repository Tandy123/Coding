## 【牛客】剑指Offer面试题

### 面试题29-顺时针打印矩阵

#### 题目描述

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

#### 分析
画图，把所有情况考虑清楚，每种情况的边界条件，书上的思路有点绕，自己也写了一个版本，总之边界条件一定要想清楚。

#### 代码

剑指Offer版：
```c
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> res;
		if(matrix.size() == 0 || matrix[0].size() == 0){
            return res;
        }
        for(int start = 0; start*2 < matrix.size() && start * 2 < matrix[0].size(); ++start){
            printMatrixInCircle(matrix, start, res);
        }
        return res;
    }
    void printMatrixInCircle(vector<vector<int> > &matrix, int start,vector<int> &res){
        int endRow = matrix.size() - 1 - start;
        int endCol = matrix[0].size() - 1 - start;
        
        for(int i = start; i <= endCol; ++i){
            res.push_back(matrix[start][i]);
        }
        if(start < endRow){
            for(int i = start+1; i <= endRow; ++i){
                res.push_back(matrix[i][endCol]);
            }
        }
        if(start < endRow && start < endCol){
            for(int i = endCol-1; i >= start ; --i){
                res.push_back(matrix[endRow][i]);
            }
        }
        if(start < endCol && start < endRow - 1){
            for(int i = endRow - 1; i>= start+1; --i){
                res.push_back(matrix[i][start]);
            }
        }
    }
};
```
我的版本，记录四个边界线，稍微好想一点
```c++
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> res;
		if(matrix.size() == 0 || matrix[0].size() == 0){
            return res;
        }
        int startRow = 0;
        int startCol = 0;
        int endRow = matrix.size()-1;
        int endCol = matrix[0].size()-1;
        while(endRow <= endRow && startCol <= endCol){
            for(int i = startCol; i <= endCol; ++i){
                res.push_back(matrix[startRow][i]);
            }
            startRow++;
            if(startRow > endRow){
                break;
            }
            for(int i = startRow; i <= endRow; ++i){
                res.push_back(matrix[i][endCol]);
            }
            endCol--;
            if(startCol > endCol){
                break;
            }
            for(int i = endCol; i >= startCol; --i){
                res.push_back(matrix[endRow][i]);
            }
            endRow--;
            if(startRow > endRow){
                break;
            }
            for(int i = endRow; i >= startRow; --i){
                res.push_back(matrix[i][startCol]);
            }
            startCol++;
        }
        return res;
    }
};
```