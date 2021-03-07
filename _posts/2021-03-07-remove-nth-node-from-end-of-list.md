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
title: 19 删除链表的倒数第 N 个结点
excerpt: 快慢指针的经典应用，比「倒数第 n 个节点」更进阶
---

## 题目

> 题目链接：[19 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

进阶：你能尝试使用一趟扫描实现吗？

示例 1：

    输入：head = [1,2,3,4,5], n = 2
    输出：[1,2,3,5]

示例 2：

    输入：head = [1], n = 1
    输出：[]

示例 3：

    输入：head = [1,2], n = 1
    输出：[1]

提示：

    链表中结点的数目为 sz
    1 <= sz <= 30
    0 <= Node.val <= 100
    1 <= n <= sz

## 思路 

1. 本题解是基于 [链表中倒数第k个节点]({% link _posts/2021-03-07-nth-node-from-end-of-list.md %}) 而来的。
2. 需注意：
   1. 因为是删除「倒数第 n 个节点」，因此，需要得到该节点的前一个节点
   2. 如果 n 为链表节点个数，该如何处理，该如何判断？
      1. 判断：快指针先走 n 步，当快指针走到 `None` 时，则走完整个链表，即 `n = 链表节点个数`；
      2. 处理：此时「删除倒数第 n 个节点」等价于「删除第一个节点」，返回第二个节点 `head.next` 即可

## 代码 

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        if not head or n == 0:
            return head

        fast, slow = head, head

        # fast 先走 n 步
        for i in range(n):
            fast = fast.next
        
        # 如果 n 刚好就是链表节点的个数
        # 那么倒数第 n 个节点就是第一个节点
        # 直接返回 head.next 即可
        if not fast:
            return head.next

        # fast 还要再走 1 步
        # 因为要删除倒数第 n 个节点
        # 所以要得到倒数第 n 个节点的前一个节点
        fast = fast.next

        while fast:
            slow, fast = slow.next, fast.next
        # 此时，slow 是倒数第 n+1 个节点
        # 删除倒数第 n 个节点直接用 slow.next.next 跳过即可
        slow.next = slow.next.next
        
        return head

```

## 分析 

1. 时间复杂度需要 `O(n)` 来遍历整个链表。
2. 空间复杂度需要 `O(1)` 来保存指针变量。