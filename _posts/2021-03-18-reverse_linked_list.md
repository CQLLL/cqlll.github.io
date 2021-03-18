---
layout: post
title:  反转链表 II
---

## [反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。

说明:
1 ≤ m ≤ n ≤ 链表长度。

### 示例1：
<pre>
<strong>输入:</strong> 1->2->3->4->5->NULL, m = 2, n = 4
<strong>输出:</strong> 1->4->3->2->5->NULL
</pre>


### 思路分析:
    举例 1 2 3 null的反转过程，1------2------3做一个基础操作1_______| 2 |_______3,说明：首先将2的next指向保存为tmp，然后指向他的前一个节点pre,

    这是2已经指向1，然后再将pre移到2，最后把1指向3，效果就是pre原本指向1，1指向2，2指向3变成pre指向2，2指向1，1指向3.

### 提交代码

```C++
class Solution {
public:
    ListNode* reverse(ListNode* head,ListNode*tail){
        ListNode * pre = head;
        ListNode * tmp;
        while(head->next!=tail){
            tmp = head->next->next;
            head->next->next = pre;
            pre = head->next;
            head->next = tmp;
        }
        return pre;
    }
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode* pre = new ListNode(0);
        ListNode* tail = head;
        pre->next = head;
        ListNode*ans = pre;
        while(--left){
            pre = pre->next;
        }
        while(right--){
            tail = tail->next;
        }
        pre->next = reverse(pre->next,tail);
        return ans->next;
    }
};
```

