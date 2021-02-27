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
title: 278 第一个错误的版本
excerpt: 二分搜索的简单使用
---

## 题目

> 题目链接：[278 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 bool isBadVersion(version) 接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

示例:

    给定 n = 5，并且 version = 4 是第一个错误的版本。

    调用 isBadVersion(3) -> false
    调用 isBadVersion(5) -> true
    调用 isBadVersion(4) -> true

    所以，4 是第一个错误的版本。 

## 思路 

> 本题采用 [二分搜索总结]({% link _posts/2021-02-20-binary-search-summary.md %}) 里的「模版 II」实现         

1. 初始化：错误的版本可能是 $[1,n]$ 区间内任意一个数，所以左右指针可取到 `1` 和 `n`。
2. 终止条件：左右指针相遇时，一定可以确定错误版本，即 `left == right`。
3. 指针移动：
   1. `if isBadVersion(mid)`：`mid` 是错误版本，那么「第一个错误版本」至少是 `mid` 以后的版本，所以 `right` 也是有可能被取到的（如果 `mid == right` 的话）
   2. `if not isBadVersion(mid)`：`mid` 不是错误版本，那么 `left` 也不会是错误版本，移动指针的时候可以向后移动。

## 代码 

```python
class Solution:
    def firstBadVersion(self, n):
        # 初始化
        left, right = 1, n

        # 终止条件
        while left < right:
            mid = (left + right) // 2
            if isBadVersion(mid):
                right = mid 
            else:
                left = mid + 1

        return left
```

## 分析 

时间复杂度需要 `O(log n)`，空间复杂度需要 `O(1)`