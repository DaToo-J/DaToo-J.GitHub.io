---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 链表 双指针 栈
title: 160 相交链表
excerpt: 链表的常见题
---

## 题目

> 题目链接：[160 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

编写一个程序，找到两个单链表相交的起始节点。


注意：

    如果两个链表没有交点，返回 null.
    在返回结果后，两个链表仍须保持原有的结构。
    可假定整个链表结构中没有循环。
    程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。

## 思路 I —— 栈

1. 如果两个链表相交，那么它们的尾巴肯定是有部分节点是重叠的。如果从尾部开始向头部扫描，最后一个相同的节点就是两个链表的交点。可是链表要怎么「从尾部向头部扫描」呢？
2. 其实，「从尾部向头部扫描」可以看作是「后进先出」的扫描，而「后进先出」是「栈」的特点。
3. 因此，可以用 2 个栈来保存链表的节点，然后再同时从栈弹出栈顶元素（即先弹出链表尾部的节点），最后一个相同的节点即为链表的交点。

## 代码 I

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        stackA, stackB = [], []
        currA, currB = headA, headB         
        
        # 用栈保存 headA 的节点
        while currA:
            stackA.append(currA)
            currA = currA.next

        # 用栈保存 headB 的节点
        while currB:
            stackB.append(currB)
            currB = currB.next

        # 防止没有交点
        res = None

        # 同时弹出链表的尾部元素
        while stackA and stackB and stackA[-1] == stackB[-1]:
            res = stackA.pop()
            stackB.pop()
        return res
```

## 分析 I

1. 时间复杂度需要 `O(n)` 来遍历链表节点。
2. 空间复杂度需要 `O(1)` 来保存链表节点，但是题目描述中说「仅用 `O(1)` 的内存，因此栈的思路仅作参考实际上可以考虑使用双指针来优化算法的空间复杂度。

## 思路 II —— 双指针

1. 如果两个链表相交，那么分别从 `headA, headB` 出发的指针 `currA, currB` 会经过一个相同的节点。但是，指针 `currA, currB` 的速度要怎么处理，才能保证它们在同一个时刻达到交点？
2. 指针 `currA, currB` 的速度差异很难得到。但是，可以先计算两个链表的长度差 `n`，更长的链表的指针先走 `n` 步，再两个指针以相同的速度前进，最终会在链表相交之处会和。

## 代码 II

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        currA, currB = headA, headB         
        # 求 headA 长度
        countA, countB = 0, 0
        while currA:
            currA = currA.next
            countA += 1

        # 求 headB 长度
        while currB:
            currB = currB.next
            countB += 1

        currA, currB = headA, headB
        for i in range(abs(countB-countA)):
            # 长度更长的链表先走 n 步
            # 此时，n = abs(countB-countA)
            if countA < countB:
                currB = currB.next
            else:
                currA = currA.next

        # 再以相同速度前进，最终会在相交之处会和
        while currA != currB:
            currA = currA.next
            currB = currB.next

        return currB
```

## 分析 II

1. 时间复杂度需要 `O(n)` 来遍历链表节点。
2. 空间复杂度需要 `O(1)` 来保存指针变量。