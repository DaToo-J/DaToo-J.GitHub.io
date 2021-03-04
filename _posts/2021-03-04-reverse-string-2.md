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
title: 541 反转字符串 II
excerpt: 「344 反转字符串」的进阶但又简单的题目
---

## 题目

> 题目链接：[541 反转字符串 II](https://leetcode-cn.com/problems/reverse-string-ii/submissions/)

给定一个字符串 s 和一个整数 k，你需要对从字符串开头算起的每隔 2k 个字符的前 k 个字符进行反转。

如果剩余字符少于 k 个，则将剩余字符全部反转。
如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。

示例:

    输入: s = "abcdefg", k = 2
    输出: "bacdfeg"

提示：

    该字符串只包含小写英文字母。
    给定字符串的长度和 k 在 [1, 10000] 范围内。


## 思路 

本题题解是在 [344 反转字符串]({% link _posts/2021-03-03-reverse-string.md %}) 的基础上修改而成。

在 [344 反转字符串]({% link _posts/2021-03-03-reverse-string.md %}) 题中已经实现了字符串的反转，本题只是反转字符串的子串，直接复用代码即可。每次反转字符串中长度为 k 的子字符串。

## 代码 

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        def reverseString(s):
            if not s: return s
            left, right = 0, len(s)-1
            while left < right:
                s[left], s[right] = s[right], s[left]
                left += 1
                right -= 1
            return s

        strs = list(s)
        for start in range(0, len(strs), k*2):
            strs[start:start+k] =  reverseString(strs[start:start+k])
        
        return ''.join(strs)
```

## 分析 

时间复杂度需要 `O(n)` 来遍历数组，空间复杂度需要 `O(1)` 来保存指针。