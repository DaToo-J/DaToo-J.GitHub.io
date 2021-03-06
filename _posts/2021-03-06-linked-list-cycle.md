---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 链表 双指针 快慢指针
title: 141 环形链表
excerpt: 快慢指针在链表题目的经典应用～
---

## 题目

> 题目链接：[141 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。

进阶：

    你能用 O(1)（即，常量）内存解决此问题吗？

## 思路 

1. 这是一个经典的快慢指针的应用。
2. 快慢指针：慢指针每次移动一步，快指针每次移动两步。
3. 如果链表有环，那么，慢指针最终会和快指针相遇。
4. 如果链表没有环，那么，快指针最终会先走出链表。

## 代码 

```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        fast, slow = head, head
        while fast and fast.next:
            # 快指针每次走两步
            fast = fast.next.next
            # 慢指针每次走一步
            slow = slow.next
            
            # 快指针和慢指针相遇，则有环
            if slow == fast:
                return True

        # 如果没有环，快指针会先走出来
        # 所以 while 的判断条件也是 fast and fast.next
        return False
```

