---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags:  链表
title: 24 两两交换链表中的节点
---

## 题目

> 题目链接：[24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例 1：

    输入：head = [1,2,3,4]
    
    输出：[2,1,4,3]

示例 2：
    
    输入：head = []
    
    输出：[]

示例 3：
    
    输入：head = [1]
    
    输出：[1]


## 思路 I


![swap-nodes-1]({{ "./assets/images/swap-nodes-1.jpg" | absolute_url }})

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

![swap-nodes-2]({{ "./assets/images/swap-nodes-2.jpg" | absolute_url }})


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

