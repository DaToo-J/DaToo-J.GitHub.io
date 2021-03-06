---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 数组 双指针 滑动窗口 前缀和 二分搜索
title: 209 长度最小的子数组
excerpt: 既可以用双指针的滑动窗口，又可以用二分法，还不错～
---

## 题目

> 题目链接：[209 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

示例 1：

    输入：target = 7, nums = [2,3,1,2,4,3]
    输出：2
    解释：子数组 [4,3] 是该条件下的长度最小的子数组。

示例 2：

    输入：target = 4, nums = [1,4,4]
    输出：1

示例 3：

    输入：target = 11, nums = [1,1,1,1,1,1,1,1]
    输出：0

提示：

    1 <= target <= 109
    1 <= nums.length <= 105
    1 <= nums[i] <= 105
 
进阶：

    如果你已经实现 O(n) 时间复杂度的解法, 请尝试设计一个 O(n log(n)) 时间复杂度的解法。

## 思路 I —— 滑动窗口 + 双指针

> 采用 **双指针** 的方法

1. 初始化“两个头”：`left, right = 0, 0`
2. 指针移动的条件：
   1. 当两个指针所截取的切片之和小于 `target` 时（即 `nums[left:right+1] < target`），向右扩大切片（两指针所切之片）的长度，即右指针向右移动一位。
   2. 当两个指针所截取的切片之和大于等于 `target` 时（即 `nums[left:right+1] >= target`），可缩小切片的长度，即左指针向右移动一位（向右靠拢）。

3. 注意：
   1. 按照以上的条件移动指针，有可能出现一直在缩小切片长度，左指针一直在向右移动，甚至越过了右指针。此时，`right < left`，应当将右指针拨到和左指针同一位置。
   2. 整个指针移动的操作应当在索引不越界的情况下进行，即 `right < len(nums) and left < len(nums)`

     ![minimum-size-subarray-sum]({{ "./assets/images/minimum-size-subarray-sum.gif" | absolute_url }}){:height="75%" width="75%"}

## 代码 I

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        left, right = 0, 0
        res = float("inf")

        while right < len(nums) and left < len(nums):
            # 防止左指针越过右指针，应将右指针拨到和左指针同一位置
            if right < left:
                right = left

            # 需扩大切片长度
            if sum(nums[left:right+1]) < target:
                right += 1
            # 需缩小切片长度，并同时更新切片长度
            else:
                res = min(res, right-left+1)
                left += 1

        return res if res != float("inf") else 0
```

## 分析 I

1. 时间复杂度：`O(n)`，其中 `n` 是数组的长度。指针 `left` 和 `right` 最多各移动 `n` 次。

2. 空间复杂度：`O(1)`。

## 思路 II —— 前缀和 + 二分搜索

1. 由题意可知，每个元素都是正数，那么数组的前缀和 **一定是递增的**，因此可以考虑使用二分搜索。
2. 那么二分搜索搜索的是啥？
   1. 可以肯定的是，只能搜索有序列表，此时是前缀和数组 `prevSum`。
   2. 待搜索的元素则是「当前遍历到的前缀和 `prevSum[i]` + `target`」。
   3. 如果搜索到的下标为 `idx`，那么要使得 `找出该数组中满足其和 ≥ target`，则需满足 `prevSum[idx] - prevSum[i] >= target`，移项后得到 `prevSum[i] + target <= prevSum[idx]`，即 `newTarget <= prevSum[idx]`。因此应该使用 `bisect_left()`。
   4. 详见：二分搜索总结的[`bisect` 模块]({% link _posts/2021-02-20-binary-search-summary.md %}) 章节

## 代码 II

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        import bisect
        
        # 求前缀和
        prevSum = [0]
        for i in nums:
            prevSum.append(prevSum[-1]+i)

        res = float("inf")
        for i in range(len(prevSum)-1):
            newTarget = target + prevSum[i]
            
            # 利用二分搜索：找更靠左的边界
            idx = bisect.bisect_left(prevSum, newTarget)

            # 防止 newTarget 超过 sum(nums)，此时只能找到最后一个位置的索引
            if idx != len(prevSum):
                res = min(res, idx - i)
        
        return res if res != float("inf") else 0
```

## 分析 II

1. 时间复杂度需要 `O(n logn)`：搜索时，先进入 `O(n)` 遍历前缀和，再花费 `O(log n)` 进行二分搜索。
2. 空间复杂度需要 `O(n)` 保存数组前缀和。