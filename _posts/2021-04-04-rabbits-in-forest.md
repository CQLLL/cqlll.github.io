---
layout: post
title:  森林中的兔子（贪心算法）
---

## [森林中的兔子](https://leetcode-cn.com/problems/trapping-rain-water-ii/)


森林中，每个兔子都有颜色。其中一些兔子（可能是全部）告诉你还有多少其他的兔子和自己有相同的颜色。我们将这些回答放在 answers 数组里。



### 示例1
<pre>
<strong>输入:</strong> answers = [1, 1, 2]
<strong>输出:</strong> 5
<strong>解释:</strong> 
两只回答了 "1" 的兔子可能有相同的颜色，设为红色。
之后回答了 "2" 的兔子不会是红色，否则他们的回答会相互矛盾。
设回答了 "2" 的兔子为蓝色。
此外，森林中还应有另外 2 只蓝色兔子的回答没有包含在数组中。
因此森林中兔子的最少数量是 5: 3 只回答的和 2 只没有回答的。
</pre>

### 示例2
<pre>
<strong>输入:</strong> answers = [10, 10, 10]
<strong>输出:</strong> 11

</pre>



### 思路分析:

每一种颜色为一个集合，求最小的兔子数量等同于最小的集合数。同一颜色集合满足数量=兔子回答+1，因此一边遍历一边统计即可。

### 提交代码

```C++

class Solution {
public:
    int numRabbits(vector<int>& answers) {
        n = answers.size();
        for(int i = 0;i < n;++i){
            a[answers[i]]++;
            if(answers[i]+1==a[answers[i]]){
                cnt += a[answers[i]];
                a[answers[i]] = 0;
            }
            
        }
        for(int i = 0;i<1001;++i){
            if(a[i]!=0){
                cnt += i+1;
            }
        }
        return cnt;
    }
private:
    int a[1001];
    int n,cnt;
};

```

