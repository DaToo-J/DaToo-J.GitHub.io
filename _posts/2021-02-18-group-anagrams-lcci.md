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
title: 面试题 10.02 变位词组
---

## 题目

> 题目链接：面试题 10.02 变位词组

编写一种方法，对字符串数组进行排序，将所有变位词组合在一起。变位词是指字母相同，但排列不同的字符串。

注意：本题相对原题稍作修改

示例:

    输入: ["eat", "tea", "tan", "ate", "nat", "bat"],
    输出:
        [
          ["ate","eat","tea"],
          ["nat","tan"],
          ["bat"]
    ]

说明：

    所有输入均为小写字母。
    不考虑答案输出的顺序。


## 思路 

变位词的组成字母都是相同的，所以可以 **按键聚合**。使用到 `collections` 类的 `defaultdict()` 方法。由于哈希表的键的类型是 **可哈希化的**，所以不能以 `list` 为键，在 python 中可以转换成 `tuple`

## 代码 

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        from collections import defaultdict
        df = defaultdict(list)
        for s in strs:
            key = sorted(s)
            df[tuple(key)].append(s)
        return list(df.values())

```
