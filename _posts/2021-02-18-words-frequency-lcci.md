---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 哈希表
title: 面试题 16.02. 单词频率
---

## 题目

> 题目链接：[面试题 16.02. 单词频率](https://leetcode-cn.com/problems/words-frequency-lcci/)

设计一个方法，找出任意指定单词在一本书中的出现频率。

你的实现应该支持如下操作：

    WordsFrequency(book)构造函数，参数为字符串数组构成的一本书
    get(word)查询指定单词在书中出现的频率

示例：

    WordsFrequency wordsFrequency = new WordsFrequency({"i", "have", "an", "apple", "he", "have", "a", "pen"});
    wordsFrequency.get("you"); //返回0，"you"没有出现过
    wordsFrequency.get("have"); //返回2，"have"出现2次
    wordsFrequency.get("an"); //返回1
    wordsFrequency.get("apple"); //返回1
    wordsFrequency.get("pen"); //返回1
提示：

    book[i]中只包含小写字母
    1 <= book.length <= 100000
    1 <= book[i].length <= 10
    get函数的调用次数不会超过100000



## 代码 

```python
class WordsFrequency:
    '''
    不用其他库函数的解法
    '''
    def __init__(self, book: List[str]):
        self.dic = {}
        for i in book:
            if i.strip() in self.dic:
                self.dic[i.strip()] += 1
            else:
                self.dic[i.strip()] = 1
        return

    def get(self, word: str) -> int:
        if word.strip() in self.dic:
            return self.dic[word.strip()]
        else:
            return 0
```

```python
class WordsFrequency:
    '''
    使用 defaultdict() 方法
    '''
    def __init__(self, book: List[str]):
        from collections import defaultdict
        self.dic = defaultdict(int)
        for i in book:
            self.dic[i.strip()] += 1

    def get(self, word: str) -> int:
        return self.dic.get(word.strip(), 0)
```