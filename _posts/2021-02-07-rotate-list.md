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


## 思路

> 题目链接：[旋转链表](https://leetcode-cn.com/problems/rotate-list/)

1. 写一个函数 `rotate(head)`：每次将「最后一个节点」和「它前一个节点」断开，再将「最后一个节点」指向「头节点」，然后重复 `k` 次即可得到答案。

2. 注意：如果 `链表长度 << k`，这样的方法容易超出时间限制，因为需要循环很多次。所以需要提前用「链表长度」对 「`k`」进行取模操作，来减少循环的次数。

## 代码

```python
class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        def getLength(head):
            # 特殊的测试用例：[1,2,3], k = 200000
            # 可以积累的经验：输入和长度有关的可以考虑是否需要取模
            curr = head
            length = 0
            while curr:
                curr = curr.next
                length += 1
            return length
        
        def rotate(head):
            dummy = head
            slow, fast = head, head.next
            while fast.next:
                fast = fast.next
                slow = slow.next
            slow.next = None
            fast.next = dummy
            return fast

        if not head or not head.next: return head
        k = k % getLength(head)
        for i in range(k):
            head = rotate(head)
        return head
```

## 分析

1. 时间复杂度需要 `O(n * (n % k)) ≈ O(n)`。
2. 空间复杂度需要 `O(1)`.