---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 基础 链表
title: 
---


## 思路 I

> 题目链接：[24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

![decode-string]({{ "./assets/images/swap-nodes-1.jpg" | absolute_url }})

## 代码 I

```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy = ListNode(-1)
        dummy.next = head
        
        pre = dummy
        cur = head

        while cur and cur.next:
            nxt = cur.next

            # 暂存 nxt.next
            tmp = nxt.next
            # 防止有环，先将 cur.next 置空
            cur.next = None

            # step 1 
            pre.next = nxt
            # step 2
            nxt.next = cur

            # step 3
            pre = cur
            # step 4
            cur = tmp

        pre.next = cur
        return dummy.next
```

## 分析 I

1. 时间复杂度需要 `O(n)`。
2. 空间复杂度需要 `O(1)`.

----




## 思路 II

![decode-string]({{ "./assets/images/swap-nodes-2.jpg" | absolute_url }})


## 代码 II

```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy = ListNode(-1)
        dummy.next = head
        prev = dummy
        while head and head.next:
            nxt = head.next
            head.next = nxt.next
            nxt.next = head
            prev.next = nxt

            prev = head
            head = head.next
        return dummy.next
```

## 分析 II

1. 时间复杂度需要 `O(n)`。
2. 空间复杂度需要 `O(1)`.

