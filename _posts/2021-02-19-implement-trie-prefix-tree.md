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
title: 208 实现 Trie (前缀树)
excerpt: 前缀树的实现，需记住里面的模版
---

## 题目

> 题目链接：[208 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。

示例:

    Trie trie = new Trie();

    trie.insert("apple");
    trie.search("apple");   // 返回 true
    trie.search("app");     // 返回 false
    trie.startsWith("app"); // 返回 true
    trie.insert("app");   
    trie.search("app");     // 返回 true

说明:

    你可以假设所有的输入都是由小写字母 a-z 构成的。
    保证所有输入均为非空字符串。

## 思路 

> 关于前缀树的总结可以看：[前缀树总结]({% link _posts/2021-02-18-trie-summary.md %})

1. 插入一个字符串 `S`：
   1. 从根节点出发，`S[0]` 是否存在于子节点中。不存在，则添加一个新的子节点。
   2. 然后到达第二个节点，并根据 `S[1]` 做出选择。 
   3. 再到第三个节点，以此类推。 
   4. 直到遍历 `S` 中的所有字符并到达末尾。 
   5. 末端节点：表示字符串 S 的节点

2. 搜索一个字符串 `S`：（搜索一个前缀也是同样的思路）
   1. 从根节点出发，`S[0]` 是否存在于子节点中。不存在，则返回 `False`。
   2. 然后到达第二个节点，并判断 `S[1]` 是否存在于子节点。 
   3. 再到第三个节点，以此类推。 
   4. 直到遍历 `S` 中的所有字符并到达末尾。 

3. 「搜索字符串」与「搜索前缀」的不同之处：
   1. 如果需要搜索字符串，需要对前缀树的节点稍作改动以标识一个单词的结尾，如上个模版的 `curr['#'] = ''`.
   2. 若只需搜索前缀，则不同特殊标识。



## 代码 

```python
class Trie:
    def __init__(self):
        self.trie = {}

    def insert(self, word: str) -> None:
        curr = self.trie
        for i in word:
            if i not in curr:
                curr[i] = {}
            curr = curr[i]
        curr['#'] = ''

    def search(self, word: str) -> bool:
        curr = self.trie
        for i in word:
            if i not in curr:
                return False
            curr = curr[i]
        if '#' not in curr:
            return False
        return True

    def startsWith(self, prefix: str) -> bool:
        curr = self.trie
        for i in prefix:
            if i not in curr:
                return False
            curr = curr[i]
        return True
```
