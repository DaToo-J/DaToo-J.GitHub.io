---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 前缀树
title: 648 单词替换
excerpt: 相比于实现前缀树，本题是前缀树的进阶应用
---

## 题目

> 题目链接：[648 单词替换](https://leetcode-cn.com/problems/replace-words/)

在英语中，我们有一个叫做 词根(root)的概念，它可以跟着其他一些词组成另一个较长的单词——我们称这个词为 继承词(successor)。例如，词根an，跟随着单词 other(其他)，可以形成新的单词 another(另一个)。

现在，给定一个由许多词根组成的词典和一个句子。你需要将句子中的所有继承词用词根替换掉。如果继承词有许多可以形成它的词根，则用最短的词根替换它。

你需要输出替换之后的句子。

示例 1：

    输入：dictionary = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"
    输出："the cat was rat by the bat"

示例 2：

    输入：dictionary = ["a","b","c"], sentence = "aadsfasf absbs bbab cadsfafs"
    输出："a a b c"

## 思路 

 大致分为 2 个步骤：
   1. 保存词根：
      1. 即前缀树的插入操作。将各个词根都插入进前缀树中。
      2. 由于需要替换「最短的词根」，所以一遇到最短的词根的结尾时，就需返回结果。
      3. 因此，需在词根结束时有一个标志。

   2. 替换继承词：
      1. 即前缀树的搜索操作。在前缀树中搜索各个继承词，找到其最短前缀。
      2. 如果当前指针 `curr` 里有结束标志，则返回前缀。
      3. 如果当前指针 `curr` 里没有当前字符，则说明没有相同前缀，直接返回字符串。

## 代码 

```python
class Solution:
    def replaceWords(self, dictionary: List[str], sentence: str) -> str:
        def insert(word):
            # 保存词根
            curr = self.trie
            for w in word:
                if w not in curr:
                    curr[w] = {}
                curr = curr[w]
            curr['#'] = word

        def replace(word):
            # 替换继承词
            curr = self.trie
            for w in word:
                if '#' in curr:
                    return curr['#']
                if w not in curr: 
                    return word
                curr = curr[w]

            # 指针结束时，需判断是否刚好是个结尾
            if '#' in curr:
                return curr['#']
            return word

        self.trie = {}
        res = ""
        for word in dictionary:
            insert(word)

        for word in sentence.split():
            res += replace(word) + " "

        return res.strip()
```


