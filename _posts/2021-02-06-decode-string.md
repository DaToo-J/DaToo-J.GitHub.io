---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags:  数组 栈
title: 394 字符串解码
excerpt: 栈的经典题目，可用于对栈加深理解。
---

## 题目

> 题目链接：[字符串解码](https://leetcode-cn.com/problems/decode-string/)

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

示例 1：

    输入：s = "3[a]2[bc]"
    输出："aaabcbc"

示例 2：

    输入：s = "3[a2[c]]"
    输出："accaccacc"

示例 3：

    输入：s = "2[abc]3[cd]ef"
    输出："abcabccdcdcdef"

示例 4：

    输入：s = "abc3[cd]xyz"
    输出："abccdcdcdxyz"

## 思路

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

