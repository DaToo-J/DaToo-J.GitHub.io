---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 回溯算法 位运算
title: 面试题 08.04. 幂集
excerpt: 
---

## 题目

> 题目链接：[面试题 08.04. 幂集](https://leetcode-cn.com/problems/power-set-lcci/)

幂集。编写一种方法，返回某集合的所有子集。集合中不包含重复的元素。

说明：解集不能包含重复的子集。

示例:

    输入： nums = [1,2,3]
    输出：
    [
      [3],
      [1],
      [2],
      [1,2,3],
      [1,3],
      [2,3],
      [1,2],
      []
    ]

## 思路 I —— 回溯

1. 回溯擅长求组合，如 `nums = [1,2,3]`，求长度为 2 的组合有：`[[1,2], [1,3], [2,3]]`。而幂集则是从长度为 `0~n` 的组合的集合。因此，可以修改一下得到以下代码。

## 代码 I —— 回溯

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        def backtracking(length, path, start):
            if len(path) == length:
                res.append(path.copy())
                return

            for i in range(start, len(nums)):
                backtracking(length, path+[nums[i]], i+1)

        res = []
        for l in range(len(nums)+1):
            # 遍历各个长度
            backtracking(l, [], 0)
        return res
```

## 思路 II —— 位运算

1. 如 `nums = [1,2,3]`，其长度为 3，那么 3 位的二进制位有：`000, 001, 010, 011, 100, 101, 110, 111`。可以用这 8 个二进制位来决定每个幂集的子元素。`0` 表示不需要对应的元素，`1` 表示留下对应的元素。

2. 比如，`000` 对应的是不需要任何元素，即 `[]`；
   
   `001` 对应的是留下最后一个元素，即 `[3]`；
   
   `011` 对应的是留下后两个元素，即 `[2,3]`；
   
   以此类推 ...


## 代码 II —— 位运算

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        # 得到子集的数量：2 ^ len(nums)
        n = 1 << len(nums)
        res = []

        for i in range(n):
            # 得到 i 的二进制表示
            cur = bin(i)[2::]
            # 补齐长度
            cur = (len(nums) - len(cur)) * '0' + cur
            tmp = []
            for idx in range(len(cur)):
                if cur[idx] != '0':
                    tmp.append(nums[idx])
            res.append(tmp.copy())
        return res
```
