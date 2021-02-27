---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags:  树 平衡二叉树
title: 110 平衡二叉树
excerpt: 考察对平衡二叉树的基本概念的掌握，递归函数在遍历中的使用。
---

## 题目

> [题目链接](https://leetcode-cn.com/problems/balanced-binary-tree/)

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1 。

## 思路 

> 节点的高度：当前节点到叶子节点的路径节点个数

1. 使用递归对二叉树进行 DFS 遍历。

2. 递归函数的作用：求得当前节点的高度。

3. 递归函数的逻辑：
   1. `当前节点的高度 = max(左孩子高度， 右孩子高度) + 1`
   2. 没有左右孩子时，高度为 1
   3. 当前节点为空节点时，高度为 0


## 代码

```python
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        def dfs(root):
            if not root: return 0
            if not root.left and not root.right: return 1
            
            left = dfs(root.left)
            right = dfs(root.right)

            if abs(left - right) > 1:
                self.res = False

            return max(left, right) + 1

        self.res = True
        if not root: return self.res
        dfs(root)
        return self.res
```

## 分析

1. 时间复杂度需要 `O(n)`，自底向上的递归，每个节点的计算高度和判断是否平衡都只需要处理一次。

2. 空间复杂度需要 `O(n)`，取决于递归调用栈的深度，递归调用的层数不超过 `n`。