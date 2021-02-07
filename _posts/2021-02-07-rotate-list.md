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

> 题目链接：[旋转链表](https://leetcode-cn.com/problems/rotate-list/)

1. 写一个函数 `rotate(head)`：每次将「最后一个节点」和「它前一个节点」断开，再将「最后一个节点」指向「头节点」，然后重复 `k` 次即可得到答案。

2. 注意：如果 `链表长度 << k`，这样的方法容易超出时间限制，因为需要循环很多次。所以需要提前用「链表长度」对 「`k`」进行取模操作，来减少循环的次数。

## 代码 I

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

## 分析 I

1. 时间复杂度需要 `O(n * (n % k)) ≈ O(n)`。
2. 空间复杂度需要 `O(1)`.


---

## 思路 II

1. 用「链表长度」对「 `k` 」取模。如果此时取模结果为 0，可以直接返回 `head`。

2. 因为要剪断「倒数第 `k+1` 个节点」和「倒数第 `k` 个节点」，所以需要找到「倒数第 `k+1` 个节点」，将问题转换为：「链表的倒数第 `n` 个节点」，即函数 `getLastKth(k)`。

3. 再对「倒数第 `k+1` 个节点」和「倒数第 `k` 个节点」进行剪剪剪✂️。


## 代码 II

```python
class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        def getLength(head):
            curr = head
            length = 0
            while curr:
                curr = curr.next
                length += 1
            return length

        def getLastKth(k):
            slow, fast = head, head
            for i in range(k):
                if fast and fast.next:
                    fast = fast.next
                else:
                    fast = head

            while fast.next:
                fast = fast.next
                slow = slow.next

            # 此时 fast 是最后一个节点
            # 此时 slow 是倒数第 k+1 个节点
            return slow, fast

        if not head or not head.next: return head
        
        # step 1: 取模
        k = k % getLength(head)
        
        # 链表长度能对 k 整除直接返回 head 即可
        if k == 0: return head

        # step 2: 倒数第 k+1 个节点 
        slow, fast = getLastKth(k)

        # step 3: 剪剪剪 ✂️
        fast.next = head
        head = slow.next
        slow.next = None

        return head

```

## 分析 II

1. 时间复杂度需要 `O(n)`。
2. 空间复杂度需要 `O(1)`.
