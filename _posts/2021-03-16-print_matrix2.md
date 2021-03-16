---
layout: post
title:  螺旋矩阵2
---

## [螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)

给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。

### 示例1：
<pre>
<strong>输入:</strong> n = 3
<strong>输出:</strong> [[1,2,3],[8,9,4],[7,6,5]]
</pre>

### 图示：

<center class="half">
    <img src="../images/leecode/spiral1.jpg"/>
</center>

### 思路分析:

- 和昨天的题目类似，但是由于方阵有很匀称的结构，边界处理可以更加随心所欲。

### 提交代码

```C++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>>ans(n,vector<int>(n));
        int left = 0,right = n-1,top = 0,bottom = n-1;
        int init = 1;
        while(2*left<n){
            for(int i = left;i<=right;++i){
                ans[top][i] = init++;
            }
            for(int i = top+1;i<=bottom;++i){
                ans[i][right] = init++;
            }
            for(int i = right-1;i>left;--i){
                ans[bottom][i] = init++;
            }
            for(int i = bottom;i>top;--i){
                ans[i][left] = init++;
            }
            left++,right--,top++,bottom--;
        }
        return ans;
    }
};
```

