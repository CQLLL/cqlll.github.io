---
layout: post
title:  链接地址法(直接用pair就完事了)
---

## [设计哈希映射](https://leetcode-cn.com/problems/design-hashmap/)

不使用任何内建的哈希表库设计一个哈希映射（HashMap）。

实现 MyHashMap 类：

- MyHashMap() 用空映射初始化对象
- void put(int key, int value) 向 HashMap 插入一个键值对 (key, value) 。如果 key 已经存在于映射中，则更新其对应的值 value 。
- int get(int key) 返回特定的 key 所映射的 value ；如果映射中不包含 key 的映射，返回 -1 。
- void remove(key) 如果映射中存在 key 的映射，则移除 key 和它所对应的 value 。

### 示例1：
<pre>
<strong>输入:</strong> ["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]

<strong>输出:</strong> [null, null, null, 1, -1, null, 1, null, -1]
</pre>

### 解释：
<pre>
MyHashMap myHashMap = new MyHashMap();
myHashMap.put(1, 1); // myHashMap 现在为 [[1,1]]
myHashMap.put(2, 2); // myHashMap 现在为 [[1,1], [2,2]]
myHashMap.get(1);    // 返回 1 ，myHashMap 现在为 [[1,1], [2,2]]
myHashMap.get(3);    // 返回 -1（未找到），myHashMap 现在为 [[1,1], [2,2]]
myHashMap.put(2, 1); // myHashMap 现在为 [[1,1], [2,1]]（更新已有的值）
myHashMap.get(2);    // 返回 1 ，myHashMap 现在为 [[1,1], [2,1]]
myHashMap.remove(2); // 删除键为 2 的数据，myHashMap 现在为 [[1,1]]
myHashMap.get(2);    // 返回 -1（未找到），myHashMap 现在为 [[1,1]]

</pre>

### 思路分析:

- 同昨天，但是今天学会用pair。

### 提交代码

```C++
#ifndef MyHashSet_H
#define MyHashSet_H

#include <bits/stdc++.h>
#include <vector>
#include <list>
#include <iostream>

using namespace std;
class MyHashMap{
private:
    vector<list<pair<int,int>>> data;
    static const int base = 803;
    static int hash(int key){
        return key%base;
    }
public:
    MyHashMap():data(base){}
    void add(int key);
    void remove(int key);
    bool contains(int key);
};
#endif


void MyHashMap::put(int key, int value) {
    int h = hash(key);
    for(auto it = data[h].begin();it != data[h].end();++it){
        if((*it).first==key){
            (*it).second = value;
            return;
        }
    }
    data[h].push_back({key,value});
}

void MyHashMap::remove(int key){
        int h = hash(key);
        for(auto it = data[h].begin();it != data[h].end();++it){
            if((*it).first==key){
                data[h].erase(it);
                return;
            }
        }
    }

// void remove(int key) {
//     int h = hash(key);
//     for(auto it = map_key[h].begin(), it2 = map_value[h].begin();it != map_key[h].end();++it,++it2){
//         if(*it==key){
//             map_key[h].erase(it);
//             map_value[h].erase(it2);
//             return;
//         }
//     }    
// }

int MyHashMap::get(int key) {
    int h = hash(key);
    for(auto it = data[h].begin();it != data[h].end();++it){
        if((*it).first==key){
            return (*it).second;
        }
    }
    return -1;
}
```

