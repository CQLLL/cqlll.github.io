---
layout: post
title:  笨阶乘（stack）
---

## [笨阶乘](https://leetcode-cn.com/problems/clumsy-factorial/)



通常，正整数 n 的阶乘是所有小于或等于 n 的正整数的乘积。例如，factorial(10) = 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1。

相反，我们设计了一个笨阶乘 clumsy：在整数的递减序列中，我们以一个固定顺序的操作符序列来依次替换原有的乘法操作符：乘法(*)，除法(/)，加法(+)和减法(-)。

例如，clumsy(10) = 10 * 9 / 8 + 7 - 6 * 5 / 4 + 3 - 2 * 1。然而，这些运算仍然使用通常的算术运算顺序：我们在任何加、减步骤之前执行所有的乘法和除法步骤，并且按从左到右处理乘法和除法步骤。

另外，我们使用的除法是地板除法（floor division），所以 10 * 9 / 8 等于 11。这保证结果是一个整数。

实现上面定义的笨函数：给定一个整数 N，它返回 N 的笨阶乘。



### 示例1
<pre>
<strong>输入:</strong> 4
<strong>输出:</strong> 7
<strong>解释:</strong> 7 = 4 * 3 / 2 + 1
</pre>



### 思路分析:

在基本计算器中，遇见过对于乘除法的压缩。这里的负号可以直接当成起始元素，运用栈可以简单的求解。

### 提交代码

```C++

class Solution {
public:
    int clumsy(int N) {
        int num = N;
        stk.push(N--);
        for(int i = 0;i<num-1;++i){
            if(i%4==0){
                auto tmp = stk.top();
                stk.pop();
                stk.push((N--)*tmp);
            }else if(i%4==1){
                auto tmp =stk.top();
                stk.pop();
                stk.push(tmp/(N--));
            }
            else if(i%4==2){
                auto tmp = stk.top();
                stk.pop();
                stk.push(tmp+(N--));
            }
            else{
                stk.push(-(N--));
            }
        }
        int ans = 0;
        while(!stk.empty()){
            auto tmp = stk.top();stk.pop();
            ans += tmp;
        }
        return ans;
    }
private:
    stack<int>stk;
};

```

