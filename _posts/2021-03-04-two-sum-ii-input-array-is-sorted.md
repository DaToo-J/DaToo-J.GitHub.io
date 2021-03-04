---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 数组 哈希表 双指针
title: 167. 两数之和 II - 输入有序数组
excerpt: 两数之和系列第二题，使用双指针或哈希表都可解出
---

## 题目

> 题目链接：[167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

给定一个已按照 升序排列  的整数数组 numbers ，请你从数组中找出两个数满足相加之和等于目标数 target 。

函数应该以长度为 2 的整数数组的形式返回这两个数的下标值。numbers 的下标 从 1 开始计数 ，所以答案数组应当满足 1 <= answer[0] < answer[1] <= numbers.length 。

你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

 
示例 1：

    输入：numbers = [2,7,11,15], target = 9
    输出：[1,2]
    解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
示例 2：

    输入：numbers = [2,3,4], target = 6
    输出：[1,3]
示例 3：

    输入：numbers = [-1,0], target = -1
    输出：[1,2]
    

提示：

    2 <= numbers.length <= 3 * 104
    -1000 <= numbers[i] <= 1000
    numbers 按 递增顺序 排列
    -1000 <= target <= 1000
    仅存在一个有效答案


## 思路 I —— 哈希表

1. 由题意可知，给定输入中，有且仅有一个有效答案。那么就可以用哈希表来实现。
2. 步骤：
   1. 将数组元素存到哈希表中：`{元素 : 元素在数组中的位置}` （ps：此处的 `位置 = 索引 + 1`）
   2. 遍历数组，如果当前元素 `numbers[i]` 和 `target - numbers[i]` 都在哈希表中，那么得到该有效答案。

## 代码 I

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        # 此处的 位置 = 索引 + 1
        dic = {item:idx+1 for  idx, item in enumerate(numbers)}
        for i in range(len(numbers)):
            if target - numbers[i] in dic:
                return [i+1, dic[target - numbers[i]]]
```

## 思路 II —— 双指针

1. 初始化一对一头一尾的双指针：`left, right`
2. 指针移动的条件：（大前提：存在有效答案，并且，输入的数组是有序的）
   1. 如果左右指针所指的元素「和为 `target`」，那么返回左右指针所在的位置。
   2. 如果左右指针所指的元素之和大于 `target`，那么说明指针之和太大，应该减少一些，又因为数组已排好序，只能将右指针向左移动，以此减小「左右指针之和」。
   3. 同理，如果左右指针所指的元素之和小于 `target`，那么需要将左指针向右移动，整体增加「左右指针之和」逐渐向 `target` 靠拢。

## 代码 II

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers)-1
        while left < right:
            if numbers[left] + numbers[right] == target:
                return [left+1, right+1]
            elif numbers[left] + numbers[right] > target:
                right -= 1
            else:
                left += 1
```

## 分析 

两种方法都：

1. 时间复杂度需要 `O(n)` 来遍历数组。
2. 空间复杂度需要 `O(n)` 来保存元素。
   