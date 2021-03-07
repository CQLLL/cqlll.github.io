---
layout: post
title: lc131-palindrome-partitioning
---


## [分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)


给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

返回 s 所有可能的分割方案。



### 示例1：
<pre>
<strong>输入:</strong> "aab"
<strong>输出:</strong> [["aa","b"], ["a","a","b"]]
</pre>

### 思路分析:

动态规划+回溯：

判断字串是否为回文串的dp数组中，由于旧有状态（i+1,j-1）位于新状态的（i,j）的左下角，所以dp数组的更新方式可以从下往上。

回溯部分类似于全排列，用递归进行暴力枚举。

### 提交代码

```C++
class Solution{
public:
void recursion(string s,int a,int b){


    if(a>b)
    {
        result.push_back(tmp);
        return;
    }
    for(int len = 1;len<=b-a+1;++len){//自a_index开始的长度
        if(dp[a][a+len-1])
        {
            tmp.push_back(s.substr(a,len));// （index，len)

            recursion(s,a+len,b);
            tmp.pop_back();
        }
    }
}
vector<vector<string>>partition(string s){
    n = s.size();
    dp.resize(n,vector<bool>(n,true));
    for(int i = n-1;i>=0;i--)
    for(int j = i+1;j<n;++j)
    {
        dp[i][j] = dp[i+1][j-1]&&(s[i]==s[j]);
    }
    recursion(s,0,n-1);
    return result;
}
private:
    vector<vector<bool>> dp;
    vector<vector<string>>result;
    vector<string>tmp;
    int n;
};



```