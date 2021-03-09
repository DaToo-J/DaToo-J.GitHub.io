---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 回溯算法 递归 双指针
title: 131 分割回文串
excerpt: 经典的回文字符串系列题目之一
---

## 题目

> 题目链接：[131 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是 回文串 。返回 s 所有可能的分割方案。

回文串 是正着读和反着读都一样的字符串。

示例 1：

    输入：s = "aab"
    输出：[["a","a","b"],["aa","b"]]

示例 2：

    输入：s = "a"
    输出：[["a"]]

提示：

    1 <= s.length <= 16
    s 仅由小写英文字母组成

## 思路 

1. 比较暴力的方式就是：将该字符串的所有分割方案都列举出来，再判断这些方案中每个子串都是回文串的方案。有以下步骤：
   1. 如果需要列出所有分割方法，最直接的方法就是「回溯算法」。利用回溯算法的模版即可求出可能的方案：
        ```python
        def bt(path, start):
            if start == len(s):
                res.append(path.copy())
                return
          
            for i in range(start, len(s)):
                path.append(s[start:i+1])
                bt(path, i+1)
                path.pop()
        ```
   2. 然后，利用双指针的方法，判断每个字符串是否是回文串：
        ```python
        def check(s):
            left, right = 0, len(s)-1
            while left < right:
                if s[left] != s[right]:
                    return False
                left += 1
                right -= 1
            return True
        ```

## 代码 

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        def check(s):
            # 判断字符串 s 是否为回文字符串
            left, right = 0, len(s)-1
            while left < right:
                if s[left] != s[right]:
                    return False
                left += 1
                right -= 1
            return True

        def bt(path, start):
            # 不同于之前的回溯，这里直接判断 start 是否已经走到尽头了
            # 因为，path 的长度不太好判断，它是数组，且由长度不同的子字符串组成。
            if start == len(s):
                res.append(path.copy())
                return
                
            for i in range(start, len(s)):
                # 如果 s[start:i+1] 不是回文字符串就不添加进去了
                if not check(s[start:i+1]): continue
                
                # 妙啊，这样可以切割字符串 s 为不同的字符串，这些子字符串的长度也可能不一致
                path.append(s[start:i+1])
                bt(path, i+1)
                path.pop()

        if not s: return []
        res = []
        bt([], 0)
        return res
```


