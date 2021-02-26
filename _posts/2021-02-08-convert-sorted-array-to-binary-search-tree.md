---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags:  树 二叉搜索树 二分搜索
title: 108 将有序数组转换为二叉搜索树
---

## 题目

> [题目链接](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:

    给定有序数组: [-10,-3,0,5,9],

    一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：

          0
         / \
       -3   9
       /   /
     -10  5

## 思路

1. 参考二分的思想：

   1. 根节点：数组的中间元素；

   2. 根节点的左孩子节点：左子数组的根节点；

   3. 根节点的右孩子节点：右子数组的根节点。

2. 这样能保证生成的树的中序遍历为有序数组，且左右子树的高度绝对值不超过 1

## 代码

```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        def binary(left, right):
            if left >= right: 
                if left not in visited:
                    # 防止漏掉边界的元素
                    return TreeNode(nums[right])
                return None
            
            mid = (left + right) // 2
            visited.add(mid)

            root = TreeNode(nums[mid])
            root.left = binary(left, mid)
            root.right = binary(mid+1, right)
            return root

        if not nums: return None
        left, right = 0, len(nums) - 1
        visited = set()
        res = binary(left, right)
        return res
```

## 分析

1. 时间复杂度需要 `O(log n)`

2. 空间复杂度需要 `O(log n)`