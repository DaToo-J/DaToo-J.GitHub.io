---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 数组 双指针
title: 485 最大连续 1 的个数
excerpt: 使用双指针（快慢指针）的方法来解此题
---

## 题目

> 题目链接：[485 最大连续 1 的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

给定一个二进制数组， 计算其中最大连续 1 的个数。

 示例：

    输入：[1,1,0,1,1,1]
    输出：3
    解释：开头的两位和最后的三位都是连续 1 ，所以最大连续 1 的个数是 3.
 

提示：

    输入的数组只包含 0 和 1 。
    输入数组的长度是正整数，且不超过 10,000。


## 思路 


1. **快慢指针**
   1. 初始化一个快指针 `fast` 和一个慢指针 `slow`；
   2. `fast`：每次都向前移动一步；
   3. `slow`：只有当 `fast` 指向 0 时，才移动到 `fast` 的位置，并在此时更新「连续 1 的个数」。

2. 需要注意：
   1. 初始化 `slow` 时，指向 `索引-1`，而不是 `索引0`。因为在更新 `slow` 时是直接跳到 `fast`，即下一个 0 的位置。因此，如果数组第一个元素是 1，并将 `slow` 指向 `索引0`就不合适。
   2. 当 `fast` 在数组中部（还未到尾元素）时，更新「连续 1 的个数」应该是：`fast - slow - 1`。即`「上一个 0」到「下一个 0」的长度 - 1`，长度需要 `-1`。
   3. 当 `fast` 到达数组尾元素时（即跳出循环），应当再判断一次，此时，「连续 1 的个数」应该是：`fast - slow`，即`「最后一个 1」到「上一个 0」的长度`，长度不用 `-1`。

## 代码 

```python
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        res = 0
        slow = -1
        for fast in range(len(nums)):
            if nums[fast] == 0:
                res = max(res, fast-slow-1)
                slow = fast
                
        # fast 指针走到数组最后一个元素时，也需要判断一次
        res = max(res, fast-slow)
        return res
```

## 分析 

1. 时间复杂度需要 `O(n)` 来遍历数组。
2. 空间复杂度需要 `O(1)` 来保存指针。