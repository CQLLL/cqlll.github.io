---
layout: post
title:  全排列I和II-子集I和II的解析
---

## [全排列](https://leetcode-cn.com/problems/permutations/)
## [全排列2](https://leetcode-cn.com/problems/permutations-ii/)
## [子集](https://leetcode-cn.com/problems/subsets/)
## [子集2](https://leetcode-cn.com/problems/subsets-ii/)


给定一个 没有重复 数字的序列，返回其所有可能的全排列。


### 全排列无重复示例：
<pre>
<strong>输入:</strong> [1,2,3]
<strong>输出:</strong> 
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
</pre>


### 全排列有重复示例：
<pre>
<strong>输入:</strong> [1,1,2]
<strong>输出:</strong> 
[[1,1,2],
 [1,2,1],
 [2,1,1]]
</pre>

### 子集无重复示例：
<pre>
<strong>输入:</strong> [1,2,3]
<strong>输出:</strong> 
[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
</pre>

### 子集有重复示例：
<pre>
<strong>输入:</strong> [1,2,2]
<strong>输出:</strong> 
[[],[1],[1,2],[1,2,2],[2],[2,2]]
</pre>

### 思路分析:

这四个练习主要是针对回溯法处理广度扩展和深度扩展的细节对比，排列问题可以归纳为对n个空格填数字，直到n个空格填满为止；
子集问题可以归纳为在小子集上不断扩充为大子集的过程；上述两者的区别反映在dfs(pos+1)还是dfs(i+1),详见代码。

### 提交代码

```C++
//全排列I
class Solution {
public:
    void dfs(vector<int>&nums,int pos){
        if(pos==n){
            res.push_back(ans);
            return;
        }
        for(int i = 0;i<n;++i){
            if(used[i]==false){ //使用过的话跳过
                used[i] = true;
                ans.push_back(nums[i]);
                dfs(nums,pos+1);
                ans.pop_back();
                used[i] = false;
            }
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        n = nums.size();
        dfs(nums,0);
        return res;
    }
private:
    int n;
    vector<int>ans;
    bool used[1000000];
    vector<vector<int>>res;
};
//全排列II
class Solution {
public:
    void dfs(vector<int>& nums,int pos){
        if(pos == n){
            res.push_back(ans);
            return;
        }
        //for循环是为了树的广度，递归是为了树的深度
        for(int i = 0;i<n;++i){
            //关键是对于重复的处理，因为进入for循环的时候，我们希望填入的数字是之前没有填过的,遇到以下情况我们希望跳过
            if(used[i]==true||(i>0&&nums[i]==nums[i-1]&&!used[i-1]))continue;//最后一个!used[i-1]很精妙，需要跳过的是同层节点有相同时候的关系
            used[i] = true;
            ans.push_back(nums[i]);
            dfs(nums,pos+1);
            used[i] = false;
            ans.pop_back();
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        //首先进行排序,可以包含重复数字
        n = nums.size();
        sort(nums.begin(),nums.end());
        dfs(nums,0);
        return res;
    }

private:
    int n;
    vector<int>ans;
    bool used[1000000];
    vector<vector<int>>res;
};


//子集I
class Solution {

    void dfs(vector<int>& nums, int pos) {
        res.push_back(ans);
        for (int i = pos; i < n; i++) {
            ans.push_back(nums[i]);
            used[i] = true;
            dfs(nums, i + 1); //这里是对小子集的扩充需求
            used[i] = false;
            ans.pop_back();
        }
    }

public:
    vector<vector<int>> subsets(vector<int>& nums) {
        n = nums.size();
        sort(nums.begin(), nums.end()); // 去重需要排序
        dfs(nums, 0);
        return res;
    }
private:
    vector<int> ans;
    vector<vector<int>> res;
    bool used[1000000];
    int n;
    
};
class Solution {

    void dfs(vector<int>& nums, int pos) {
        res.push_back(ans);
        for (int i = pos; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) { //树同层的相同则跳过
                continue;
            }
            ans.push_back(nums[i]);
            used[i] = true;
            dfs(nums, i + 1);
            used[i] = false
            ans.pop_back();
        }
    }

public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        n = nums.size();
        sort(nums.begin(), nums.end()); // 去重需要排序
        dfs(nums, 0);
        return res;
    }
private:
    vector<int> ans;
    vector<vector<int>> res;
    bool used[1000000];
    int n;
    
};

```

