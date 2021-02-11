---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: BFS 队列 广度优先
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

1. 写一个函数 `bfs(x, y)`，其作用是：遍历以 `(x, y)` 为起始节点（即 `grid[x][y] = "1"`）的岛屿的所有节点。防止下次还会遍历这些节点，需将这些节点都置 `"0"`。

2. 遍历整个 `gird`，如遇到 `grid[x][y] = "1"`，则调用 `bfs(x, y)` 函数。由于在 `bfs(x, y)` 函数中已将访问过的节点置为 `"0"`，因此在遍历 `grid` 时只会遍历到不同岛屿的节点，也就是说调用过几次 `bfs(x, y)` 函数就有几个岛屿。

## 代码 

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        import queue

        def bfs(x, y):
            q = queue.Queue()
            q.put((x,y))
            while q.qsize() > 0:
                row, col = q.get()
                for dx, dy in [(1,0), (-1,0), (0,1), (0,-1)]:
                    if 0 <= row+dx < len(grid) and 0 <= col+dy < len(grid[0]) and grid[row+dx][col+dy] == "1":
                        q.put((row+dx, col+dy))
                        # 防止重复遍历，需置 0
                        grid[row+dx][col+dy] = "0"

        res = 0
        for r in range(len(grid)):
            for c in range(len(grid[0])):
                if grid[r][c] == "1":
                    bfs(r, c)
                    res += 1
        return res
```

## 分析 

1. 时间复杂度需要 `O(m * n)`，`m, n` 分别为 `grid` 的高和宽
2. 空间复杂度需要 `O(m * n)`