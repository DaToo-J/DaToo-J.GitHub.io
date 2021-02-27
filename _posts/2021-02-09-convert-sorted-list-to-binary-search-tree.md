---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags:  链表 二叉搜索树
title: 109 有序链表转换二叉搜索树
excerpt: 本题与 「108 将有序数组转换为二叉搜索树」有异曲同工之妙，只不过将输入从数组变成了链表
---

## 题目

> 题目链接：[109 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)


给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:

    给定的有序链表： [-10, -3, 0, 5, 9],

    一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：

          0
         / \
       -3   9
       /   /
     -10  5



## 思路 

1. 本题与 [108 将有序数组转换为二叉搜索树]({% link _posts/2021-02-08-convert-sorted-array-to-binary-search-tree.md %}) 有异曲同工之妙，只不过将输入从数组变成了链表

2. 在 [108 将有序数组转换为二叉搜索树]({% link _posts/2021-02-08-convert-sorted-array-to-binary-search-tree.md %}) 题中，采用的是二分法的思路，但是链表不像数组有索引，可以通过索引得到数组的中间元素。

3. 但是可以通过快慢指针来得到链表的中间节点，作为树的根节点，然后递归调用该函数得到左右子树(子链表)的根节点。

4. 当要对左右子链表的中间节点进行查询时，可以对快指针 `fast` 进行判断，得知是否达到链表的结尾。


## 代码 

```python
class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        def binary(start, end):
            if not start or start == end: return None

            # 快慢指针
            fast, slow = start, start
            while fast and fast.next and fast != end and fast.next != end:
                fast = fast.next.next
                slow = slow.next

            root = TreeNode(slow.val)
            root.left = binary(start, slow)
            root.right = binary(slow.next, end)
            
            return root
        
        if not head: return None
        return binary(head, None)
```

## 分析 

1. 时间复杂度需要 `O(n * log n)`。因为二分能将查询次数降为 `O(log)`，但每次查询都需要快慢指针对子链表进行全部遍历，需要 `O(n)`。

2. 空间复杂度需要 `O(n)`，用于保存所有节点。