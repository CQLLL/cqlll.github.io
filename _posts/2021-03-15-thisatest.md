---
layout: post
title:  this is a test
---

## [螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

给你一个`m`行`n`列的矩阵`matrix`，请按照`顺时针螺旋顺序`，返回矩阵中的所有元素。

### 示例1：
<pre>
<strong>输入:</strong> matrix = [[1,2,3],[4,5,6],[7,8,9]]
<strong>输出:</strong> [1,2,3,6,9,8,7,4,5]
</pre>

### 图示：

<center class="half">
    <img src="../images/leecode/spiral1.jpg"/>
</center>

### 思路分析:

- 设置left right top bottom四个边界变量，考虑按层收缩（一层代表一个外围），注意矩阵角落的数值不要重复输出。

### 提交代码

```C++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        //假定矩阵n行m列
        int n = matrix.size();
        if(!n)return {};
        int m = matrix[0].size();
        int left = 0,right = m-1,top = 0,bottom = n-1;
        while(left<=right&&top<=bottom){
            for(int i = left;i<=right;++i){
                ans.push_back(matrix[top][i]);
            }
            for(int i = top+1;i<=bottom;++i){
                ans.push_back(matrix[i][right]);
            }
            if(left<right&&top<bottom)
            {
                for(int i = right-1;i>=left;--i){
                    ans.push_back(matrix[bottom][i]);
                }
                for(int i = bottom-1;i>top;--i){
                    ans.push_back(matrix[i][left]);
                }
            }
            left++;
            right--;
            top++;
            bottom--;
        }
        return ans;
    }
private:
    vector<int> ans;
};
```

