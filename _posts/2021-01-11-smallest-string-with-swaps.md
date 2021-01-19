---
layout: post
title: LC1202
---
## [交换字符串中的元素]](https://leetcode-cn.com/problems/smallest-string-with-swaps/)

给你一个字符串 s，以及该字符串中的一些「索引对」数组 pairs，其中 pairs[i] = [a, b] 表示字符串中的两个索引（编号从 0 开始）。

你可以 任意多次交换 在 pairs 中任意一对索引处的字符。

返回在经过若干次交换后，s 可以变成的按字典序最小的字符串。



### 示例1：
<pre>
<strong>输入:</strong> s = "dcab", pairs = [[0,3],[1,2]]
<strong>输出:</strong> "bacd"
<strong>解释:</strong> 
交换 s[0] 和 s[3], s = "bcad"
交换 s[1] 和 s[2], s = "bacd"
</pre>

### 示例2：
<pre>
<strong>输入:</strong> s = "dcab", pairs = [[0,3],[1,2],[0,2]]
<strong>输出:</strong> "abcd"
<strong>解释:</strong> 
交换 s[0] 和 s[3], s = "bcad"
交换 s[0] 和 s[2], s = "acbd"
交换 s[1] 和 s[2], s = "abcd"

</pre>

### 思路分析:

多个pairs对可以构成一个连通量，在一个连通量中的任意字母可以相互调换，也就是可以存在任意序列。那么，我们只要找到这些不相交的连通量集合，对每个连通量集合进行排序即可，用到的数据结构为Disjoint-Set。

### 并查集模板
```C++
class Djset{
public:
    vector<int>parent;
    vector<int>rank;
    int count;
    Djset(int n):count(n){
        for(int i = 0;i<count;++i){
            parent.push_back(i);
            rank.push_back(0);
        }
    }
    // 用秩较大的树合并秩较小的树，可以减小路径压缩的压力
    int Find(int x){
        if(parent[x]!=x){
            parent[x] = Find(parent[x]);
        }
        return parent[x];
    }
    void Link(int rx,int ry){
        if(rank[rx]>rank[ry]){
            parent[ry] = rx;
        }
        else{
            parent[rx] = ry;
            if(rank[rx]==rank[ry])
                rank[ry] = rank[ry]+1; 
        }
    }
    void Unin(int x,int y){
        Link(Find(x),Find(y));
    }
};


```

```C++
class Solution {
public:
    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        int n = s.size();
        vector<char> rs(n);
        Djset ds(n);
        for (const auto& e : pairs) ds.Unin(e[0], e[1]);
        
        //  格式化并查集
        unordered_map<int, vector<int> > um;
        for (int i = 0; i < n; i++) um[ds.Find(i)].push_back(i);
        
        // 同一并查集按字典序排序
        for (auto& [k, v] : um) {
            vector<int> c = v;
            int v_len = v.size();
            sort(v.begin(), v.end(), [&](auto a, auto b) {
                return s[a] < s[b];
            });

            for (int i = 0; i < c.size(); i++) rs[c[i]] = s[v[i]];
        }
        s = "";
        for (const auto& e : rs) s += e;
        return s;
    }
};
```