---
layout: post
title:  接雨水 II(优先队列+BFS)
---

## [接雨水II](https://leetcode-cn.com/problems/trapping-rain-water-ii/)


给你一个 m x n 的矩阵，其中的值均为非负整数，代表二维高度图每个单元的高度，请计算图中形状最多能接多少体积的雨水。

<center class="half">
    <img src="../images/leecode/rainwater_empty.png"/>
</center>



<center class="half">
    <img src="../images/leecode/rainwater_fill.png"/>
</center>




### 示例1
<pre>
<strong>输入:</strong> [
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]
<strong>输出:</strong> 4
</pre>



### 思路分析:

借鉴双指针的思路，从短边开始搜索，可以保证搜索得到的方格可以装下max(0,当前最短-当前所在)。这里只需要用优先队列来求得这个最短即可。

### 提交代码

```C++
class Solution {
public:
    int dx[4] = {-1,1,0,0};
    int dy[4] = {0,0,-1,1};
    int trapRainWater(vector<vector<int>>& heightMap) {
        priority_queue<PIII,vector<PIII>,greater<PIII>> pq;
        int n = heightMap.size(),m = heightMap[0].size();
        vector<vector<bool>> st = vector<vector<bool>> (n,vector<bool>(m,false));
        if(n<=2||m<=2)return 0;
        for(int i = 0;i < m;++i){
            st[0][i] = true;
            st[n-1][i] = true;
            pq.push({heightMap[0][i],{0,i}});
            pq.push({heightMap[n-1][i],{n-1,i}});
        }
        for(int i = 1;i<n-1;++i){
            st[i][0] = true;
            st[i][m-1] = true;
            pq.push({heightMap[i][0],{i,0}});
            pq.push({heightMap[i][m-1],{i,m-1}});
        }
        int ans  = 0;
        while(!pq.empty()){
            auto sml = pq.top();
            pq.pop();
            for(int i = 0;i<4;++i){
                int x = sml.second.first+dx[i];
                int y = sml.second.second+dy[i];
                if(x>=0&&x<n&&y>=0&&y<m&&!st[x][y]){
                    ans += max(0,sml.first-heightMap[x][y]);
                    st[x][y] = true;
                    pq.push({max(sml.first,heightMap[x][y]),{x,y}});
                }
            }
        }
        return ans;

    }
};


```

