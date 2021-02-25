---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 二分搜索
title: 658 找到 K 个最接近的元素
---

## 题目

> 题目链接：[658 找到 K 个最接近的元素](https://leetcode-cn.com/problems/find-k-closest-elements/)

给定一个排序好的数组 arr ，两个整数 k 和 x ，从数组中找到最靠近 x（两数之差最小）的 k 个数。返回的结果必须要是按升序排好的。

整数 a 比整数 b 更接近 x 需要满足：

    |a - x| < |b - x| 或者
    |a - x| == |b - x| 且 a < b
 
示例 1：

    输入：arr = [1,2,3,4,5], k = 4, x = 3
    输出：[1,2,3,4]

示例 2：

    输入：arr = [1,2,3,4,5], k = 4, x = -1
    输出：[1,2,3,4]

提示：

    1 <= k <= arr.length
    1 <= arr.length <= 104
    数组里的每个元素与 x 的绝对值不超过 104

## 思路 

> 本解法采用 [二分搜索总结]({% link _posts/2021-02-20-binary-search-summary.md %}) 里的「模版 I」实现         

1. 大体思路：
   1. 先求出 `x` 在目标数组 `arr` 的索引 `idx`。
   2. 可以确定一个子数组的区间：即 `idx` 前后 `2k` 个元素，`arr[idx-k: idx+k]`。
   3. 从这个子数组的两头开始搜索，根据题意进行判断，直到子数组的长度为 `k`。

2. `search_idx()`：
   1. 和正常的二分搜索一样
   2. 在后处理时，需判断 `left` 是否是真的搜索到索引为 `0` 的元素，还是该 `x` 不存在。

3. `search_sublist(idx)`：
   1. 初始化：`left, right` 分别是 `idx-k, idx+k`，只不过需要注意索引的边界
   2. 循环条件：确保 `left, right` 所截取的子数组长度在等于 `k` 时，跳出循环。
   3. 返回值：当跳出循环时，`left = right - 1 - k`，需返回 `arr[left: right-1]` 或 `arr[left: left+k]`

## 代码 

```python
class Solution:
    def findClosestElements(self, arr: List[int], k: int, x: int) -> List[int]:
        def search_idx():
            # 常规的二分搜索
            left, right = 0, len(arr)-1
            while left <= right:
                mid = (left + right) // 2
                if arr[mid] == x:
                    return mid
                elif arr[mid] < x:
                    left = mid + 1
                else:
                    right = mid - 1

            if left == 0 and arr[left] != x:
                return -1
            return left

        def search_sublist(idx):
            # 初始化：需注意 left 和 right 的在数组索引的边界
            left, right = max(idx - k, 0), min(idx + k, len(arr)-1)
            
            # 循环跳出时，left 和 right 之间有 k 个元素
            while (right - left + 1 ) > k:
                if abs(arr[right] - x) >= abs(arr[left]-x):
                    right -= 1
                else:
                    left += 1

            return arr[left: left+k]

        idx = search_idx()
        return search_sublist(idx)
```

## 分析 

1. 需进行两次二分搜索，需要时间 `O(log n)`。
2. 空间需要 `O(1)`。