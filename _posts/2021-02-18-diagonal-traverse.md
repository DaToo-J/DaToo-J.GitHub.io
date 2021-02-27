---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 数组
title: 498 对角线遍历
---

## 题目

> 题目链接：[498 对角线遍历](https://leetcode-cn.com/problems/diagonal-traverse/)

给定一个含有 M x N 个元素的矩阵（M 行，N 列），请以对角线遍历的顺序返回这个矩阵中的所有元素，对角线遍历如下图所示。

示例:

    输入:
        [
         [ 1, 2, 3 ],
         [ 4, 5, 6 ],
         [ 7, 8, 9 ]
        ]

    输出:  [1,2,4,7,5,3,6,8,9]

说明:

    给定矩阵中的元素总数不会超过 100000 。

## 思路 

1. 矩阵的「上对角」与其没有来回反向的对角线如下图。通过分析数组的索引特点，可以很好的发现这些对角线的规律：
   1. 起始节点都是第一行的节点
   2. 每条对角线都是从右上角往左下角移动

      ![queue]({{ "./assets/images/diagonal-traverse-1.png" | absolute_url }}){:height="50%" width="50%"}

    ```python
    # 伪代码：

    for c in range(N):
      # 初始节点
      row, col = 0, c
      
      # 存对角线上的元素
      tmp = []
      res = []

      while 一些限制条件:
          tmp.append(matrix[row][col])
          row += 1
          col -= 1
      res.append(tmp)
    ```

2. 矩阵的「下对角」与其对角线如下图。这些对角线的规律：
   1. 起始节点都是最后一列的节点
   2. 每条对角线都是从右上角往左下角移动

      ![queue]({{ "./assets/images/diagonal-traverse-2.png" | absolute_url }}){:height="50%" width="50%"}

    ```python
    # 伪代码：
    for r in range(1, M):
      # 初始节点
      row, col = r, N-1
      
      # 存对角线上的元素
      tmp = []

      while 一些限制条件:
          tmp.append(matrix[row][col])
          row += 1
          col -= 1
      res.append(tmp)
   ```
3. 此时，`res` 里面的元素应该是：`[[1], [2, 4], [3, 5, 7], [6, 8, 4], [9, 5, 7], [6, 8], [9]]`，实际上的结果应该是：`[1,2,4,7,5,3,6,8,4,7,5,9,6,8,9]`。因此，需要将 `res` 中索引为偶数的子数组翻转。并且合并整个 `res` 数组。

   ![queue]({{ "./assets/images/diagonal-traverse-3.png" | absolute_url }}){:height="50%" width="50%"}

## 代码 

```python
class Solution:
    def findDiagonalOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix: return matrix

        M, N = len(matrix), len(matrix[0])
        if not M or not N: return matrix
        
        res = []
        count = 0
        
        # 上对角
        for c in range(N):
            row, col = 0, c
            tmp = []
            # 限制条件
            while 0 <= row < M and 0 <= col < N:
                tmp.append(matrix[row][col])
                row += 1
                col -= 1
            count += 1
            
            # 反转对角线
            if count % 2 == 1:
                res.extend(tmp[::-1])
            else:
                res.extend(tmp[:])

        # 下对角
        for r in range(1, M):
            row, col = r, N-1
            tmp = []
            # 限制条件
            while 0 <= row < M and 0 <= col < N:
                tmp.append(matrix[row][col])
                row += 1
                col -= 1
            count += 1

            # 反转对角线
            if count % 2 == 1:
                res.extend(tmp[::-1])
            else:
                res.extend(tmp[:])
        return res
```

## 分析 

1. 时间复杂度需要 `O(M * N)` 去遍历整个矩阵。
2. 空间复杂度需要 `O(M * N)` 保存所有元素。