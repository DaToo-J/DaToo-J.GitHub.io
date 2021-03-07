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
title: 142 环形链表 II
excerpt: 快慢指针在链表题目的经典应用～
---

## 题目

> 题目链接：[142 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

说明：不允许修改给定的链表。

进阶：

    你是否可以使用 O(1) 空间解决此题？
 

## 思路 


![linked-list-cycle-ii.jpeg]({{ "./assets/images/linked-list-cycle-ii.jpeg" | absolute_url }})


1. 主要思路：先使用快慢指针，再使用双指针
2. 步骤：
   1. 初始化快慢指针：`fast, slow = head, head`
      1. 如果 `fast` 或 `fast.next` 为空，则不存在环。
      2. 否则，`fast` 会和 `slow` 相遇。
   2. 双指针：再将一个新的指针指向 `head`，并同时 **以相同的速度**（每次都走一步）和慢指针从各自的位置出发，即新指针在 `head`，慢指针在相遇点。当快慢指针再次相遇时，第二个相遇点即为环的入口。
3. 证明：
   1. 记：
      1. 头节点到入口点的距离为：$a$
      2. 入口点到相遇点的距离为：$b$
      3. 相遇点到入口点的距离为：$c$
   2. 第一次相遇：快慢指针从头节点出发
      1. 快指针：每次走 2 步
      2. 慢指针：每次走 1 步
      3. 快指针走过：$a + k(b+c) + b$ （解释：快指针在与慢指针相遇之前，一直会在环里绕圈圈，共绕 k 圈。因此其路程有 k(b+c)）
      4. 慢指针走过：$a + b$
   3. 推导： 
        
        ∵ 快指针的速度是慢指针的 2 倍 
        
        ∴ 路程也是 2 倍，即 $a + k(b+c) + b = 2(a+b)$ 

        ∴ 移项后得到：$a = (k-1)(b+c) + c$

        ⇒ 由上式可知，如果求得等号右边 $(k-1)(b+c) + c$，则可得等号左边 $a$，则可以得到 「从头节点到入口点的距离」

        ⇒ 等号右边的物理意义（$a$ 的长度的特点）：在环里走 $k-1$ 圈，再走过距离为 $c$ 的路。那么，刚好是「从相遇点到入口点」的距离。

        ⇒ 因此，需要「第二次相遇」
    1. 第二次相遇：第三个指针从头节点出发，慢指针从相遇点出发
       1. 第三个指针：每次走 1 步
       2. 慢指针：每次走 1 步
       3. 当「第三个指针」和「慢指针」相遇时，此相遇点为环的入口点，bingo～

## 代码 

```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        fast, slow = head, head
        while True:
            # 没有环
            if not fast or not fast.next: return

            fast = fast.next.next
            slow = slow.next
            
            # 第一次相遇：快慢指针的相遇
            # 应该在向移动之后再判断，否则初始化时 fast 就等于 slow
            if fast == slow: break

        # 初始化一个新的指针指向 head
        third = head
        while True:
            # 第二次相遇：third 和 slow 指针相遇
            if third == slow: return slow
            third = third.next
            slow = slow.next
```

## 分析 

1. 时间复杂度需要 `O(n)` 来遍历整个链表。
2. 空间复杂度需要 `O(1)` 来保存指针变量。