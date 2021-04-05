---
layout: post
title:  面试题 17.21. 直方图的水量(单调栈和双指针)
---

## [面试题 17.21. 直方图的水量](https://leetcode-cn.com/problems/volume-of-histogram-lcci/)


给定一个直方图(也称柱状图)，假设有人从上面源源不断地倒水，最后直方图能存多少水量?直方图的宽度为 1。

<center class="half">
    <img src="../images/leecode/rainwatertrap.png"/>
</center>


### 示例1
<pre>
<strong>输入:</strong> [0,1,0,2,1,0,1,3,2,1,2,1]
<strong>输出:</strong> 6
</pre>



### 思路分析:

单调栈方法：
    当维护一个单调递减栈时，一旦发现新的圆柱高度大于栈顶元素这时就会产生装水（为零的情况是栈顶元素到栈底元素都相同）。
    那么可以在维护弹出元素的时候计算装水量。




双指针方法：
    首先定义左右的两堵高墙leftmax和rightmax，双指针定位left和right,当我们发现nums[left]和nums[right]有大小区别时，假设两者身边有更高的屏障保护，则一定会产生装水,优先短板，此时依赖于短板对应高墙的高度（这个机制可以保证短板对应高墙高度-短板高度一定可以形成装水）。

### 提交代码

```C++

class Solution {
public:
    int trap(vector<int>& nums) {
        n = nums.size();
        int ans = 0;
        for(int i = 0;i<n;++i){
            while(!stk.empty()&&nums[stk.top()]<nums[i]){
                int tmp = stk.top();
                stk.pop();
                while(!stk.empty()&&nums[stk.top()]==nums[tmp]){
                    int tmp = stk.top();
                    stk.pop();
                }
                if(stk.empty())break;
                int left = stk.top();
                ans += (min(nums[left],nums[i])-nums[tmp])*(i-left-1);
            }
            stk.push(i);
        }
        return ans;
    }
private:
    int n;
    stack<int>stk;
};

class Solution{
public:
    int trap(vector<int>& nums) {
        //双指针法
        //当我们发现left和right有大小区别时，假设两者身边有更高的屏障保护，则一定会产生装水
        n = nums.size();
        int left = 0,right = n-1;
        int leftmax = 0,rightmax = 0;
        int ans = 0;
        while(left<right){
            leftmax = max(leftmax,nums[left]);
            rightmax = max(rightmax,nums[right]);
            if(nums[right]>nums[left]){
                ans += leftmax-nums[left];
                left++;
            }else{
                ans += rightmax - nums[right];
                right--;
            }
        }
        return ans;
    }
private:
    int n;
};
```

