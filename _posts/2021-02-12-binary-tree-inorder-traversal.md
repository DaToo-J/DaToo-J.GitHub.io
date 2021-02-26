---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 二叉树 遍历 中序遍历 栈
title: 94 二叉树的中序遍历
excerpt: 二叉树的必须掌握的遍历方式
---

## 题目

> 题目链接：[94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

## 思路 

1. 中序遍历：`左 - 根 - 右`

2. 栈的实现方式：先用指针找到每颗子树的最左下角，然后进行进出栈操作

## 代码 

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        if not root: return []
        stack = []
        curr = root
        res = []
        while stack or curr:
            while curr:
                stack.append(curr)
                curr = curr.left
            curr = stack.pop()
            res.append(curr.val)
            curr = curr.right
        return res
```


