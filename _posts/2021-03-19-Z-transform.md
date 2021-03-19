---
layout: post
title:  Z 字形变换
---

## [Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

<pre>
P   A   H   N
A P L S I I G
Y   I   R
</pre>
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。

请你实现这个将字符串进行指定行数变换的函数：
<pre>
string convert(string s, int numRows);
</pre>


### 示例1：
<pre>
<strong>输入:</strong> s = "PAYPALISHIRING", numRows = 3
<strong>输出:</strong> "PAHNAPLSIIGYIR"
</pre>


### 思路分析:

为了充分利用其中的数学规律，我们将原始的字符串补全为Z单元的整数倍，这样对于每个i都可以映射为新字符串的新位置。


### 提交代码

```C++
class Solution {
public:
    string convert(string s, int numRows) {
        // s预先填充
        n = s.size();
        int part_nums = 2*numRows-2; 
        if(part_nums==0)return s;
        if((n%part_nums))
            s = s+string(part_nums-n%part_nums,'#');
        int parts = s.size()/part_nums;
        string ans(s.size(),'#');

        for(int i = 0;i<n;++i){
            if(i%part_nums==0){
                ans[i/part_nums] = s[i];
            }
            else if(i%part_nums==numRows-1){
                ans[(2*(numRows-1)-1)*parts+i/part_nums] = s[i];
            }
            else{
                int yushu = i%part_nums;
                int part = i/part_nums;
                int cnt = yushu<part_nums-yushu?0:1;
                yushu = min(yushu,part_nums-yushu);
                ans[(2*yushu-1)*parts+part*2+cnt] = s[i];
            }
        }
        string res = "";
        for(int i = 0;i<ans.size();i++){
            if(ans[i]!='#')
                res +=ans[i];
        }
        return res;
    }
private:
    int n;
};

//思路更为清晰的解法：https://leetcode-cn.com/problems/zigzag-conversion/solution/z-zi-xing-bian-huan-by-leetcode/

class Solution{
public:
    string convert(string s, int numRows){
        if(numRows == 1)return s;

        string ret;
        int n = s.size();
        int cycleLen = 2 * numRows -2;

        for(int i = 0;i < numRows; ++i){
            for(int j = 0; j+i < n;j+= cycleLen){
                ret += s[i+j];
                if(i != 0 && i != numRows - 1 && j + cycleLen -i < n)
                    ret += s[j+cycleLen-i];
            }
        }
        return ret;
    }
}

```

