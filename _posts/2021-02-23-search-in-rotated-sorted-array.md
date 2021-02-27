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
title: 33 搜索旋转排序数组
---

## 题目

> 题目链接：[33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的索引，否则返回 -1 。

示例 1：

    输入：nums = [4,5,6,7,0,1,2], target = 0
    输出：4

示例 2：

    输入：nums = [4,5,6,7,0,1,2], target = 3
    输出：-1

示例 3：

    输入：nums = [1], target = 0
    输出：-1

提示：

    1 <= nums.length <= 5000
    -10^4 <= nums[i] <= 10^4
    nums 中的每个值都 独一无二
    nums 肯定会在某个点上旋转
    -10^4 <= target <= 10^4
    
进阶：你可以设计一个时间复杂度为 O(log n) 的解决方案吗？


## 思路 

> 欢迎访问我在 [LeetCode 上的题解](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/solution/3-zhang-tu-dai-ni-tong-guo-ci-ti-by-dato-mw50/)
> 
> 本题采用 [二分搜索总结]({% link _posts/2021-02-20-binary-search-summary.md %}) 里的「模版 I」实现         

1. 根据「旋转点」的位置，可分为两种情况：靠左或靠右。 不管怎么旋转，旋转后的数组都有以下特点：
   1. `蓝色数组的最小值 > 黄色数组的最大值`，即 `所有蓝色数组的元素 > 所有黄色数组的元素`；
   2. 蓝色数组长度和黄色数组长度孰长孰短，是动态变化的，取决于指针的移动。
   ![rotated-array-1]({{ "./assets/images/rotated-array-1.jpeg" | absolute_url }}){:height="75%" :width="75%"}

2. 再根据「整体数组的中间值」和「蓝黄长度对比」，`target` 的落点可分为以下 6 种情况：
   1. 当旋转点靠右时：（`target` 可落在 ①~③）
      1. 如果 `target` 落在了 `1` 区：则 `nums[mid] > target`。因为 `nums[mid]` 和 `target` 属于同一数组，`target` 更靠左，就更小。
      2. 如果 `target` 落在了 `2` 区：则 `nums[mid] < target`。因为 `nums[mid]` 和 `target` 属于同一数组，`target` 更靠右，就更大。
      3. 如果 `target` 落在了 `3` 区：则 `nums[mid] > target`。因为 `nums[mid]` 属于蓝色数组，`target` 属于黄色数组，并且 `所有蓝色数组的元素 > 所有黄色数组的元素`。
   2. 当旋转点靠左时：（`target` 可落在 ④~⑥）
      1. 如果 `target` 落在了 `4` 区：则 `nums[mid] < target`。因为 `nums[mid]` 属于黄色数组，`target` 属于蓝色数组，并且 `所有黄色数组的元素 > 所有蓝色数组的元素`。
      2. 如果 `target` 落在了 `5` 区：则 `nums[mid] > target`。因为 `nums[mid]` 和 `target` 属于同一数组，`target` 更靠左，就更小。
      3. 如果 `target` 落在了 `6` 区：则 `nums[mid] < target`。因为 `nums[mid]` 和 `target` 属于同一数组，`target` 更靠右，就更大。
   ![rotated-array-2]({{ "./assets/images/rotated-array-2.jpeg" | absolute_url }}){:height="75%" :width="75%"}

3. 转换为代码的思路：（指针的移动）
   1. 当旋转点靠右时，如果 `target` 落在了 `1` 区，那么右指针 **一定会向左移动**。只需判断 $target$ 是否处于 $[nums[left], nums[mid]]$ 区间内即可。
   1. 同理。当旋转点靠左时，如果 `target` 落在了 `6` 区，那么左指针 **一定会向右移动**。只需判断 $target$ 是否处于 $[nums[mid], nums[right]]$ 区间内即可。
   ![rotated-array-3]({{ "./assets/images/rotated-array-3.jpeg" | absolute_url }}){:height="75%" :width="75%"}

## 代码 

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        # 初始化
        left, right = 0, len(nums)-1

        # 终止条件
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid

            # 旋转点靠右
            if nums[mid] >= nums[left]:
                # target 落在 1 区
                if nums[left] <= target <= nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1

            # 旋转点靠左
            else:
                # target 落在 6 区
                if nums[mid] <= target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1

        return -1
```

## 分析 

使用二分搜索可将复杂度降为 `O(log n)`