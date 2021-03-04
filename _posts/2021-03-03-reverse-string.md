---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 双指针
title: 344 反转字符串
excerpt: 双指针的简单应用
---

## 题目

> 题目链接：[344 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

示例 1：

    输入：["h","e","l","l","o"]
    输出：["o","l","l","e","h"]
示例 2：

    输入：["H","a","n","n","a","h"]
    输出：["h","a","n","n","a","H"]

## 思路 

> 图源来自 LeetCode 官方
> 相关题目：[541 反转字符串 II]({% link _posts/2021-03-04-reverse-string-2.md %}) 

![two-pointers]({{ "./assets/images/two-pointers.gif" | absolute_url }}){:height="75%" width="75%"}
 
一头一尾双指针，两两交换元素，然后再逐渐向中间靠拢。

## 代码 

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        if not s: return s
        left, right = 0, len(s)-1
        while left < right:
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1
        return s
```

## 分析 

时间复杂度需要 `O(n)` 来遍历数组，空间复杂度需要 `O(1)` 来保存指针。