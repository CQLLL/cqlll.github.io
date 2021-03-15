---
layout: post
title: regular expression(常规表示法)
---

## 正则表达式

正则表达式基本是一种表示法，支持正则表达式的工具awk、grep、vi、sed等，完成过滤、分析工作,日常使用vim做文字编辑或程序编写时使用到的（查找、替换），需要配合正则表达式，找出该目录下的mail。cp、ls等命令并未支持正则表达式，所以就只能使用bash自己本身的通配符而已。

- 查找文件下的带mail字符串的文件
> grep 'mail' /lib/systemd/system/* 

- 屏蔽不良邮件

- 基础正则表达式和扩展正则表达式
  

### 基础正则表达式

对字符排序有影响的语系数据就会对正则表达式的结果有影响。

#### 语系对正则表达式的影响

> LANG=C 0 1 2 3 4...A B C D...Z a b c d ...z
> LANG=zh_CN 0 1 2 3 4...a A b B c C d D ...z Z

使用[A-Z]对于两者会有区别

#### awk 

awk 后面接两个单引号并加上大括号{}来设置想要对数据进行的处理操作，awk可以处理后续接的文件，也可以读取前个命令的标准输出。

写一个bash脚本以统计一个文本文件words.txt中每个单词出现的频率。

为了简单起见，你可以假设：
- words.txt 只包括小写字母和' '。
- 每个单词只由小写字母组成。
- 单词间由一个或多个空格字符分隔。

> awk '{for(i=1;i<=NF;i++>){nums[$i]++;}};END{for(w in nums){print w,nums[w];}}' words.txt | sort -rn -k2