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
title: 剑指 Offer 22 链表中倒数第k个节点
excerpt: 快慢指针的经典应用
---

## 题目

> 题目链接：[剑指 Offer 22 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

示例：

    给定一个链表: 1->2->3->4->5, 和 k = 2.

    返回链表 4->5.



## 思路 

1. 步骤：
   1. 快指针：先从头节点出发，向前移动 `k` 个节点
   2. 快指针和慢指针同时以 **每次 1 步** 的速度前进，当快指针达到链表末尾时，慢指针所在之处即为「倒数第 k 个节点」

## 代码 

```python
class Solution:
    def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:
        if not head or k == 0:
            return 

        latter, former = head, head
        for i in range(k):
            former = former.next 
        
        while former:
            latter, former = latter.next, former.next
        return latter
```

## 分析 

1. 时间复杂度需要 `O(n)` 来遍历整个链表。
2. 空间复杂度需要 `O(1)` 来保存指针变量。