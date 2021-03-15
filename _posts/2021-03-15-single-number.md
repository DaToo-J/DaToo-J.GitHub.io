---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 位运算 数学
title: 136 只出现一次的数字
excerpt: 「只出现一次的数字」系列之一
---

## 题目

> 题目链接：[136 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

    输入: [2,2,1]
    输出: 1

示例 2:

    输入: [4,1,2,1,2]
    输出: 4

## 思路 I —— 位运算

1. 在 [位运算总结]({% link _posts/2021-03-03-bit-manipulation-summary.md %}) 中，可知异或运算的性质：
   1. $a ⊕ a = 0$：任何数和其自身进行异或，等于 0

   2. $a ⊕ 0 = a$：任何数和0进行异或，等于其本身

2. 利用这两条性质，本题的解题思路可以是：对数组的每个元素都进行异或运算，相同的数字进行异或运算后为 0，可以抵消出现了两次的数字。最终析出只出现了一次的数字。

## 代码 I

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        res = 0
        for i in nums:
            res ^= i
        return res
```

## 思路 II —— 数学

1. 用数学的方法：$去重后的和 * 2 - 未去重的和$

## 代码 II

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        return sum(set(nums)) * 2 - sum(nums)
```
