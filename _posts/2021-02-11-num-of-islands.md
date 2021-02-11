---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: DFS 栈 深度优先
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
> 
> DFS 解法的递归实现见：[岛屿数量]({% link _posts/2021-02-11-numbers-of-islands.md %}) 

1. 由 [岛屿数量——DFS 解法的递归实现]({% link _posts/2021-02-11-numbers-of-islands.md %}) 可知，DFS 的遍历顺序为：
   1. 根节点（在这里是 `grid[r][c] == "1"` 的起始节点）
   2. 根节点的上方节点 `A`
   3. 以该节点 `A` 为根节点的上方节点
   4. 直到这个方向达到最大深度
   5. 再回溯到根节点，遍历根节点的其他方向节点，以此循环。

2. 本题解的 DFS 不使用递归的方式，而是使用栈的方式来实现。
   1. 将根节点入栈
   2. 出栈时，先判断当前节点是否符合「岛屿」的定义
   3. 符合，则将其上下左右方向上的节点入栈
   4. 重复 2-3 步骤，直到栈为空

3. 由于栈的「后入先出」特性，以此循环，只会将后进入的节点，先弹出。而每次入栈的顺序都是固定的（上下左右），因此，每次出栈都会先弹出一个方向上的节点，直到达到最大深度。然后再弹出其他方向上的节点，类似于回溯的作用。


## 代码 

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        def isValid(row, col):
            return 0 <= row < R and 0 <= col < C and grid[row][col] == "1"

        def dfs(r, c):
            stack = [(r, c)]
            while stack:
                row, col = stack.pop()
                if not isValid(row, col):
                    continue

                grid[row][col] = "0"
                stack.append((row-1, col))
                stack.append((row+1, col))
                stack.append((row, col-1))
                stack.append((row, col+1))

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