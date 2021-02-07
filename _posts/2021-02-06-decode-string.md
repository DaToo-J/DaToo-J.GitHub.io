---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 基础 数组 栈
title: 
---

## 思路

> 题目链接：[字符串解码](https://leetcode-cn.com/problems/decode-string/)

![decode-string]({{ "./assets/images/decode-string.png" | absolute_url }})

## 代码

```python
class Solution:
    def decodeString(self, s: str) -> str:
        stack = []
        count = 0
        tmp = ''

        for i in s:
            if '0' <= i <= '9':
                count = int(i) + count * 10
            elif i == '[':
                stack.append((count, tmp))
                tmp, count = '', 0
            elif i == ']':
                currCount, lastTmp = stack.pop()
                tmp = lastTmp + currCount * tmp
            else:
                tmp += i
        return tmp
```

## 分析

1. 时间复杂度需要 `O(n)`， 空间复杂度需要 `O(1)`

