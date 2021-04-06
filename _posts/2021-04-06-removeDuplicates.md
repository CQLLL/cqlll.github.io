---
layout: post
title:  删除有序数组中的重复项 II
---

## [删除有序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 最多出现两次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。


### 示例1
<pre>
<strong>输入:</strong> nums = [1,1,1,2,2,3]
<strong>输出:</strong> 5, nums = [1,1,2,2,3]
<strong>解释:</strong> 
函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。 不需要考虑数组中超出新长度后面的元素。
</pre>



### 思路分析:

迭代器模拟遍历:
由于重复长度只需满足不超过2即可。首先想到的是可以设置一个开关，这个开关在发现前后相等的时候打开，然后在开关打开的状态下发现重复就删除。

快慢指针：
如果允许重复的长度为K,那么快慢指针的方法更具有泛化能力,基于判断第slow-max_repeat+1和fast是不是相等，来决定slow的区域扩容。


### 提交代码

```C++


//迭代器取巧
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        for(auto it = nums.begin()+1;it!=nums.end();++it){
            if(*it==*(it-1)){
                if(on_off){
                    it--;
                    nums.erase(it);
                }
                on_off = true;
            }
            else{
                on_off = false;
            }
        }
        return nums.size();
    }
private:
    bool on_off = false;
};


//快慢指针解法(更具有泛化性)
class Solution{
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size()<=2)return nums.size();
        int max_repeat = 2;
        int slow = max_repeat - 1;//定位到索引为1的位置
        for(int fast = slow+1;fast<nums.size();++fast){
            if(nums[fast]!=nums[slow-max_repeat+1]){
                slow++;
                nums[slow] = nums[fast];
            }
        } 
        return slow+1;
    }
};


```

