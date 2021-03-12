---
layout: post
title:  栈在二叉树前序遍历中的特殊应用
---

## [验证二叉树的前序序列化](https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/)

序列化二叉树的一种方法是使用前序遍历。当我们遇到一个非空节点时，我们可以记录下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如 #。

```
     _9_
    /   \
   3     2
  / \   / \
 4   1  #  6
/ \ / \   / \
# # # #   # #

```

例如，上面的二叉树可以被序列化为字符串 "9,3,4,#,#,1,#,#,2,#,6,#,#"，其中 # 代表一个空节点。

给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不重构树的条件下的可行算法。

每个以逗号分隔的字符或为一个整数或为一个表示 null 指针的 '#' 。

你可以认为输入格式总是有效的，例如它永远不会包含两个连续的逗号，比如 "1,,3" 。




### 示例1：
<pre>
<strong>输入:</strong> "9,3,4,#,#,1,#,#,2,#,6,#,#"
<strong>输出:</strong> true
</pre>

### 示例2：
<pre>
<strong>输入:</strong> "1,#"
<strong>输出:</strong> false
</pre>

### 思路分析:

- 根据题意，二叉树前序遍历会造成最后两个字符为“#”，那么可以合并“x”，“#”，“#”为“#”，直到栈中只有一个“#”。

- 从入度和出度的角度考虑，对root初始化`度`（degree）为1，每当遇到“#”，入度-1，每当遇到数字，入度-1出度+2，如果当前`度`为零，则表明树是不存在的（因为数字可以使得`度`增加，但是出现先了过量的消耗，例如“#，#，#”）。

### 提交代码

```C++

class Solution {
public:
    bool isValidSerialization(string preorder) {
        vector<string> s;
        int n = preorder.size();
        string digit = "";
        vector<string>::iterator it;
        int i = 0;
        while(i<n)
        {
            if(isdigit(preorder[i]))
            {
                digit+=preorder[i];
                i++;
                if(!isdigit(preorder[i])||i==n){
                    s.push_back(digit);
                    digit="";
                }
                continue;
            }
            if(!isdigit(preorder[i])&&preorder[i]!=',')
            {
                s.push_back("#");
                i++;
            }
            while(s.size()>=3&&*(it=(s.end()-1))=="#"&&*(it-1)=="#"&&*(it-2)!="#")
            {
                s.pop_back();s.pop_back();s.pop_back();
                s.push_back("#");
            }
            i++;
        }
        return s.size()==1 && s.back() == "#";
    }
};

class Solution{
public:
    bool isValidSerialization(string pre){
        int n = pre.size();
        int i = 0;
        int degree = 1;
        while(i < n){
            if(degree == 0)
                return false;
            if(pre[i]==',')
                i++;
            else if(pre[i]=='#'){
                i++;
                degree--;
            }
            else{
                while(i<n&&pre[i]!=','){
                    i++;
                }
                degree++;
            }
        }
        return !degree;
    }
};


```

