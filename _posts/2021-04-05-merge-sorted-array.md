---
layout: post
title:  合并两个有序数组
---

## [合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。你可以假设 nums1 的空间大小等于 m + n，这样它就有足够的空间保存来自 nums2 的元素。




### 示例1
<pre>
<strong>输入:</strong> nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
<strong>输出:</strong> [1,2,2,3,5,6]
<strong>解释:</strong> 

</pre>



### 思路分析:

由于nums1已经是扩充好容量的数组，所以可以进行原址操作，定位指针设置在末尾，同时nums2没有填充完整即表明需要填充到头部。

### 提交代码

```C++

class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int last_pos = m+n-1,first = m-1,second = n-1;
        while(second>=0&&first>=0){
            if(nums2[second]>=nums1[first]){
                nums1[last_pos--] = nums2[second--];
            }
            else{
                nums1[last_pos--] = nums1[first--];
            }
        }
        for(int i = 0;i<=second;++i){
            nums1[i] = nums2[i];
        }
    }
private:
    int n1,n2;
};

```

