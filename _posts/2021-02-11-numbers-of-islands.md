---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: DFS 递归 深度优先
title: 200 岛屿数量
---

## 题目

> 题目链接：[200 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

 

示例 1：

    输入：grid = [
      ["1","1","1","1","0"],
      ["1","1","0","1","0"],
      ["1","1","0","0","0"],
      ["0","0","0","0","0"]
    ]

    输出：1

示例 2：

    输入：grid = [
      ["1","1","0","0","0"],
      ["1","1","0","0","0"],
      ["0","0","1","0","0"],
      ["0","0","0","1","1"]
    ]
    输出：3
 

提示：

    m == grid.length
    n == grid[i].length
    1 <= m, n <= 300
    grid[i][j] 的值为 '0' 或 '1'



## 思路 

> BFS 解法见：[岛屿数量]({% link _posts/2021-02-09-number-of-islands.md %}) 

1. 依然延续 [岛屿数量 BFS解法]({% link _posts/2021-02-09-number-of-islands.md %}) 的思路，只不过换成 DFS 的遍历方法。

2. 遍历顺序：
   1. 根节点（在这里是 `grid[r][c] == "1"` 的起始节点）
   2. 根节点的上方节点 `A`
   3. 以该节点 `A` 为根节点的上方节点
   4. 直到这个方向达到最大深度
   5. 再回溯到根节点，遍历根节点的其他方向节点，以此循环。

3. 注意：
   1. 使用本题解使用递归回溯实现
   2. 终止条件：遇到不符合「岛屿」定义的节点就 `return`。
   3. 搜索空间：当前节点的上下左右方向上的节点。
   4. 特殊处理：防止重复遍历节点，需要将已遍历过的「岛屿」置为 `"0"`

## 代码 

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        def isValid(row, col):
            return 0 <= row < R and 0 <= col < C and grid[row][col] == "1"

        def dfs(row, col):
            # 终止条件
            if not isValid(row, col):
                return
            
            # 防止重复遍历
            grid[row][col] = "0"
            # 搜索空间
            dfs(row - 1, col)
            dfs(row + 1, col)
            dfs(row, col - 1)
            dfs(row, col + 1)
            return 

        R, C = len(grid), len(grid[0])
        count = 0
        for r in range(R):
            for c in range(C):
                # 找到岛屿的起始节点
                if grid[r][c] == "1":
                    dfs(r, c)
                    count += 1
        return count
```

## 分析 

1. 时间复杂度需要 `O(m * n)`，`m, n` 分别为 `grid` 的高和宽
2. 空间复杂度需要 `O(m * n)`