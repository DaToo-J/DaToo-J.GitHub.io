---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 二分搜索
title: 34 在排序数组中查找元素的第一个和最后一个位置
---

## 题目

> 题目链接：[34 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

进阶：

    你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？
 
示例 1：

    输入：nums = [5,7,7,8,8,10], target = 8
    输出：[3,4]

示例 2：

    输入：nums = [5,7,7,8,8,10], target = 6
    输出：[-1,-1]

示例 3：

    输入：nums = [], target = 0
    输出：[-1,-1]
 

提示：

    0 <= nums.length <= 105
    -109 <= nums[i] <= 109
    nums 是一个非递减数组
    -109 <= target <= 109

## 思路 I

> ![binary-search]({{ "./assets/images/binary-search.jpg" | absolute_url }}){:height="75%" :width="75%"}
> 本解法采用 [二分搜索总结]({% link _posts/2021-02-20-binary-search-summary.md %}) 里的「模版 III」实现         

1. 大体思路：先求出 `target` 在 `nums` 的最左边的那个索引，再求出 `target` 在 `nums` 的最右边的那个索引，若没找到则返回 `-1`。然后将左右索引组成数组作为结果返回。
2. 求最左边的索引：`searchLeft()`
   1. 初始化：左右指针指向数组的开始和结束
   2. 循环条件：`left + 1 < right` 确保搜索空间至少有 2 个元素，当循环退出时，只剩余 2 个元素，需对这 2 个元素进行后处理。
   3. 后处理：此时只剩 2  个元素，且 `left` 和 `right` 是相邻的。因为要找 `target` 「最左的索引」，所以先判断 `nums[left]` 是否等于 `target`，再判断 `nums[right]` 是否等于 `target`。若都不是，则返回 `-1`。
3. 求最右边的索引：`searchRight()`
   1. 初始化和循环条件同理于 `searchLeft()`
   2. 后处理：因为要找「最右的索引」，先判断 `nums[right]` 是否等于 `target`，再判断 `nums[left]` 是否等于 `target`。

## 代码 I

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        def searchLeft():
            left, right = 0, len(nums)-1
            while left + 1 < right:
                mid = (left + right) // 2
                # 和 searchRight() 的不同之处
                if nums[mid] >= target:
                    right = mid  
                else:
                    left = mid 

            # 和 searchRight() 的不同之处：先判断 left，再判断 right
            if left < len(nums) and nums[left] == target:
                return left
            elif right >= 0 and nums[right] == target:
                return right
            else:
                return -1

        def searchRight():
            left, right = 0, len(nums)-1
            while left + 1 < right:
                mid = (left + right) // 2
                # 和 searchLeft() 的不同之处
                if nums[mid] <= target:
                    left = mid 
                else:
                    right = mid 

            # 和 searchLeft() 的不同之处：先判断 right，再判断 left
            if right >= 0 and nums[right] == target:
                return right
            elif left < len(nums) and nums[left] == target:
                return left
            else:
                return -1

        if not nums: return [-1, -1]
        l = searchLeft()
        r = searchRight()

        return [l, r]
```

## 分析 I

1. 需要 2 次遍历数组，每次遍历的时间复杂度因二分切割需要 `O(log n)`。
2. 空间复杂度需要 `O(1)`，常数个变量即可。


## 思路 II

> 本解法与上一个思路不同，采用 [二分搜索总结]({% link _posts/2021-02-20-binary-search-summary.md %}) 里的「模版 I」实现         
> 
> 参考「剑指 Offer 53 - I. 在排序数组中查找数字 I」

1. 二分搜索：搜索的是 `target` 的右边界，再搜索 `target - 1` 的右边界
   - 例子：`nums = [5,7,7,8,8,10], target = 8`
      1. 输入 `target, 8`：返回 `8` 的右边界，`5`
      2. 输入 `target-1, 7`：返回 `7` 的右边界， `3`
      3. 最终结果：`[target-1 的右边界,  target 的右边界 - 1]`
2. 初始化：数组的开始和结尾
3. 循环条件：见注释
4. 后处理：可 `return left`，也可 `return right`，见注释。
        


## 代码 II

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        def search(target):
            left, right = 0, len(nums)-1
            while left <= right:
                mid = (left + right) // 2
                # 如果 mid 大于 target 那么右指针要向左移动
                if nums[mid] > target:
                    right = mid - 1
                # 如果 nums[mid] == target，左指针向 mid 右移动一位，那么就会越过这个 target
                # 不过，正好也是要查询 target 的右边界，那么即便是左指针右移跨过某个 target 也没关系
                else:
                    left = mid + 1

            # 跳出循环时：left == right - 1
            # left：数组中最右边 target 的下一个元素的索引，所以 return 的是 [l, r-1]
            # right：数组中最右边 target 的索引，所以 return 的是 [l+1, r]
            return left

        if not nums: return [-1, -1]
        r = search(target)
        l = search(target - 1)
        if l == r:
            return [-1, -1]
        return [l, r-1]
```        

## 分析 II

1. 与「分析 I」同理