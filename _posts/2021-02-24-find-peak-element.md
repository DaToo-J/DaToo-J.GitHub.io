---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 二分搜索
title: 162 寻找峰值
---

## 题目

> 题目链接：[162 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)

峰值元素是指其值大于左右相邻值的元素。

给你一个输入数组 nums，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 任何一个峰值 所在位置即可。

你可以假设 nums[-1] = nums[n] = -∞ 。

示例 1：

    输入：nums = [1,2,3,1]
    输出：2
    解释：3 是峰值元素，你的函数应该返回其索引 2。

示例 2：

    输入：nums = [1,2,1,3,5,6,4]
    输出：1 或 5 
    解释：你的函数可以返回索引 1，其峰值元素为 2；
         或者返回索引 5， 其峰值元素为 6。
    
提示：

    1 <= nums.length <= 1000
    -231 <= nums[i] <= 231 - 1
    对于所有有效的 i 都有 nums[i] != nums[i + 1]
 


## 思路 

> 本题采用 [二分搜索总结]({% link _posts/2021-02-20-binary-search-summary.md %}) 里的「模版 II」实现         

1. 初始化：可以取到数组任意一个数，所以左右指针为 `1` 和 `len-1`。
2. 终止条件：左右指针相遇，即 `left == right`。
3. 指针移动：二分搜索原本是针对 **有序数组的查找**，根据「左右指针中间位置的值」和「左右指针指向的值」去移动指针。但是本题是对「部分有序的数组」进行查找，根据题意返回其中一个峰值即可。而峰值是数组内子数组的最大值，虽然数组不是有序的，但是该子数组是有序的，可以通过「左右指针中间位置的值」和「中间位置下一个数」来判断。
   1. 当 `nums[mid] <= nums[mid+1]`：说明 `mid` 处于一个上升的有序数组，向右移动左指针能达到其中一个峰值。
   2. 当 `nums[mid] > nums[mid+1]`：说明 `mid` 至少已经越过了一个上升的有序数组，将右指针左移缩小范围就能找到越过的上升有序数组的峰值。


## 代码 

```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        # 初始化
        left, right = 0, len(nums) - 1

        # 终止条件
        while left < right:
            mid = (left + right) // 2
            if nums[mid] <= nums[mid+1]:
                left = mid + 1
            else:
                right = mid 
        return left
```

## 分析 

时间复杂度需要 `O(log n)`，空间复杂度需要 `O(1)`