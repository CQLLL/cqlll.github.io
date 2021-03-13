---
layout: post
title:  链接地址法(gcc不包含iostream？)
---

## [设计哈希集合](https://leetcode-cn.com/problems/design-hashset/)

不使用任何内建的哈希表库设计一个哈希集合（HashSet）。

实现 MyHashSet 类：

- void add(key) 向哈希集合中插入值 key 。
- bool contains(key) 返回哈希集合中是否存在这个值 key 。
- void remove(key) 将给定值 key 从哈希集合中删除。如果哈希集合中没有这个值，什么也不做。

### 示例1：
<pre>
<strong>输入:</strong> ["MyHashSet", "add", "add", "contains", "contains", "add", "contains", "remove", "contains"]
[[], [1], [2], [1], [3], [2], [2], [2], [2]]

<strong>输出:</strong> [null, null, null, true, false, null, true, null, false]
</pre>

### 解释：
<pre>
MyHashSet myHashSet = new MyHashSet();
myHashSet.add(1);      // set = [1]
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(1); // 返回 True
myHashSet.contains(3); // 返回 False ，（未找到）
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(2); // 返回 True
myHashSet.remove(2);   // set = [1]
myHashSet.contains(2); // 返回 False ，（已移除）

</pre>

### 思路分析:

- 根据算法导论散列表的链接法，hash函数对一个比较大的质数求取余数，然后通过链表查找来增减元素(直接开数组并不可取)。

### 提交代码

```C++
#ifndef MyHashSet_H
#define MyHashSet_H

#include <bits/stdc++.h>
#include <vector>
#include <list>
#include <iostream>

using namespace std;
class MyHashSet{
private:
    vector<list<int>> data;
    static const int base = 803;
    static int hash(int key){
        return key%base;
    }
public:
    MyHashSet():data(base){}
    void add(int key);
    void remove(int key);
    bool contains(int key);
};
#endif



void MyHashSet::add(int key){
        int h = hash(key);
        for(auto it = data[h].begin();it != data[h].end();++it){
            if(*it==key){
                return;
            }
        }
        data[h].push_back(key);
}

void MyHashSet::remove(int key){
        int h = hash(key);
        for(auto it = data[h].begin();it != data[h].end();++it){
            if(*it==key){
                data[h].erase(it);
                return;
            }
        }
    }

bool MyHashSet::contains(int key){
    int h = hash(key);
    for(auto it = data[h].begin();it!=data[h].end();it++){
        if(*it==key){
            return true;
        }
    }
    return false;
}


```

