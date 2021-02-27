---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 哈希表
title: 954 二倍数对数组
excerpt: 数学在算法中的简单应用
---

## 题目

> 题目链接：[954 二倍数对数组](https://leetcode-cn.com/problems/array-of-doubled-pairs/)

给定一个长度为偶数的整数数组 arr，只有对 arr 进行重组后可以满足 “对于每个 0 <= i < len(arr) / 2，都有 arr[2 * i + 1] = 2 * arr[2 * i]” 时，返回 true；否则，返回 false。

示例 1：

    输入：arr = [3,1,3,6]
    输出：false

示例 2：

    输入：arr = [2,1,2,6]
    输出：false

示例 3：

    输入：arr = [4,-2,2,-4]
    输出：true
    解释：可以用 [-2,-4] 和 [2,4] 这两组组成 [-2,-4,2,4] 或是 [2,4,-2,-4]

示例 4：

    输入：arr = [1,2,4,16,8,4]
    输出：false

提示：

    0 <= arr.length <= 3 * 104
    arr.length 是偶数
    -105 <= arr[i] <= 105

## 思路 

> 参考 LeetCode 官方

1. 按照元素的绝对值大小进行遍历：
   1. 如果 `i` 尚未被使用，那么 `i` 和 `i*2` 都需要在 `arr` 中，如果没有 `i*2` 就返回 `False`。
   2. 如果所有元素都被访问过，那么返回 `True`

## 代码 

```python
class Solution:
    def canReorderDoubled(self, arr: List[int]) -> bool:
        count = collections.Counter(arr)

        for i in sorted(arr, key=abs):
            # 如果 i 为负数，且 arr 没有 -i 就会为 0
            # 或者之前已经被访问过（减为 0），说明已经配对成功
            if count[i] == 0: continue

            # i 尚未被使用 且 没有 i*2 就返回 False
            if count[i*2] == 0: return False
            
            # i 和 i*2 都存在，就计数减一
            count[i] -= 1
            count[i*2] -= 1
        return True
```

## 分析 

1. 时间复杂度需要 `O(n)` 来对 `arr` 遍历。
2. 空间复杂度需要 `O(n)` 来对 `arr` 计数。
