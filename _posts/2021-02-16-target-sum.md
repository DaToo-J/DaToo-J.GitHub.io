---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: DFS 深度优先
title: 494 目标和
---

## 题目

> 题目链接：[494 目标和](https://leetcode-cn.com/problems/target-sum/)

给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

示例：

    输入：nums: [1, 1, 1, 1, 1], S: 3
    输出：5
    解释：
        -1+1+1+1+1 = 3
        +1-1+1+1+1 = 3
        +1+1-1+1+1 = 3
        +1+1+1-1+1 = 3
        +1+1+1+1-1 = 3
    一共有5种方法让最终目标和为3。 

提示：

    数组非空，且长度不会超过 20 。
    初始的数组的和不会超过 1000 。
    保证返回的最终结果能被 32 位整数存下。


## 思路 & 代码 

 这题最开始的思路是：用回溯的方法穷尽所有组合，然后统计达到 `S` 的个数。然后有以下的代码：

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        def dfs(curSum, idx):
            if idx == len(nums):
                if curSum == S:
                    self.res += 1
                return

            dfs(curSum + nums[idx], idx+1)
            dfs(curSum - nums[idx], idx+1)
        
        self.res = 0
        dfs(0, 0)
        return self.res
```

以上的代码虽然思路是对的，但是<u>可能会超过时间限制</u>。因为有些的计算是重复的，称之 **重叠子问题**，是可以不用重复计算，可以使用 **备忘录** 记录计算过的数。可将以上代码改写为以下代码优化时间：

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        def dfs(curSum, idx):
            if idx == len(nums):
                if curSum == S:
                    return 1
                return 0

            if (idx, curSum) in self.memo:
                return self.memo[(idx, curSum)]

            res = dfs(curSum + nums[idx], idx+1) + dfs(curSum - nums[idx], idx+1)
            self.memo[(idx, curSum)] = res
            return res

        self.visited = {}
        self.memo = {}
        return dfs(0, 0)
        
```

