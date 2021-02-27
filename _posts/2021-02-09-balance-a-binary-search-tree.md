---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags:  二叉搜索树 二叉平衡树
title: 1382 将二叉搜索树变平衡
---

## 题目

> 题目链接：[1382 将二叉搜索树变平衡](https://leetcode-cn.com/problems/balance-a-binary-search-tree/)

给你一棵二叉搜索树，请你返回一棵 平衡后 的二叉搜索树，新生成的树应该与原来的树有着相同的节点值。

如果一棵二叉搜索树中，每个节点的两棵子树高度差不超过 1 ，我们就称这棵二叉搜索树是 平衡的 。

如果有多种构造方法，请你返回任意一种。

示例：

    输入：root = [1,null,2,null,3,null,4,null,null]
    输出：[2,1,3,null,null,null,4]
    解释：这不是唯一的正确答案，[3,1,4,null,2,null,null] 也是一个可行的构造方案。

## 思路 

1. 在 [树的总结]({% link _posts/2021-02-04-tree-summary.md %}) 里有总结到，二叉搜索树的特点：
   
   1. $左孩子 < 根节点, 右孩子 > 根节点$
   
   2. 中序遍历结果是一个有序列表

2. 所以可以先将二叉搜索树的节点通过中序遍历得到，然后将问题转换为「将有序数组转换为二叉搜索平衡树」，在 108 题的题解（[将有序数组转换为二叉搜索树]({% link _posts/2021-02-08-convert-sorted-array-to-binary-search-tree.md %}) ）中，可以通过二分法将有序数组转化为二叉搜索树时保持平衡。

## 代码 

```python
class Solution:
    def balanceBST(self, root: TreeNode) -> TreeNode:
        def inorder(root):
            # 中序遍历
            if not root: return

            inorder(root.left)
            stack.append(root.val)
            inorder(root.right)

        def binary(start, end):
            # 有序数组转二叉搜索树
            if start >= end: 
                if start not in visited:
                    return TreeNode(stack[start])
                return 

            mid = (start + end) // 2
            visited.add(mid)
            root = TreeNode(stack[mid])
            root.left = binary(start, mid)
            root.right = binary(mid+1, end)

            return root

        stack = []
        inorder(root)
        visited = set()
        return binary(0, len(stack)-1)
```

## 分析 

1. 时间复杂度需要 `O(log n)`，空间复杂度需要 `O(n)` 用于保存节点。