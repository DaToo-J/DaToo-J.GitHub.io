---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 前缀树 总结
title: 前缀树总结
---

## 一、概述

> ![trie]({{ "./assets/images/trie.png" | absolute_url }}){:height="30%" width="50%"}

1. 前缀树，又称 **字典树**，是 **N 叉树** 的特殊形式。

2. 主要作用：存储字符串

3. 结构特点：
   1. 每个节点有多个子节点
   2. 通向不同子节点的路径上有不同的字符
   3. 根节点：空字符串
   4. 每一个节点：一个字符子串，即前缀。由「通往该子节点路径上所有字符」和「节点本身的原始字符串」组成的。
   5. 节点的所有后代：都有着 **共同前缀**

4. 实质：利用「空间换时间」。

   1. 如果想在一堆单词中找某个单词或者某个前缀是否出现，我无需进行完整遍历，而是遍历前缀树即可。
   2. 本质上，使用前缀树和不使用前缀树，减少的时间就是公共前缀的数目。
   3. 也就是说，一堆单词没有公共前缀，使用前缀树没有任何意义。如果题目中公共前缀比较多，可以考虑用前缀树来优化。

5. 应用场景：
   1. 自动补全
   2. 拼写检查


## 二、实现

1. 通常使用 **哈希表** 来实现，因为：
   1. 访问特点节点较为容易
   2. 只需存储子节点，节省空间
   3. 更加灵活，不受固定长度和固定范围的限制

2. 如果只需要在前缀树中存储单词，那么可能需要：
   1. 在每个节点中声明一个标志，表明该节点所表示的字符串是否为一个单词
   2. 或者，在单词结束的节点用一个标志标识单词的结束


## 三、基本操作

### 1. 插入

1. 插入一个字符串 `S`：
   1. 从根节点出发，`S[0]` 是否存在于子节点中。不存在，则添加一个新的子节点。
   2. 然后到达第二个节点，并根据 `S[1]` 做出选择。 
   3. 再到第三个节点，以此类推。 
   4. 直到遍历 `S` 中的所有字符并到达末尾。 
   5. 末端节点：表示字符串 S 的节点

2. 模版：
    ```python
    def insert(self, word: str) -> None:
        curr = self.trie
        for i in word:
            if i not in curr:
                curr[i] = dict()
            curr = curr[i]
        curr['#'] = ''
    ```

### 2. 搜索

1. 搜索一个字符串 `S`：（搜索一个前缀也是同样的思路）
   1. 从根节点出发，`S[0]` 是否存在于子节点中。不存在，则返回 `False`。
   2. 然后到达第二个节点，并判断 `S[1]` 是否存在于子节点。 
   3. 再到第三个节点，以此类推。 
   4. 直到遍历 `S` 中的所有字符并到达末尾。 

2. 「搜索字符串」与「搜索前缀」的不同之处：
   1. 如果需要搜索字符串，需要对前缀树的节点稍作改动以标识一个单词的结尾，如上个模版的 `curr['#'] = ''`.
   2. 若只需搜索前缀，则不同特殊标识。

3. 「搜索字符串」模版：
    ```python
    def search(self, word: str) -> bool:
        # 搜索字符串
        curr = self.trie
        for i in word:
            if i not in curr:
                return False
            curr = curr[i]
        if '#' not in curr:
            return False
        return True
    ```

4. 「搜索前缀」模版：
    ```python
    def startsWith(self, prefix: str) -> bool:
        curr = self.trie
        for i in prefix:
            if i not in curr:
                return False
            curr = curr[i]
        return True
    ```

## 四、相关题目

| No  | Problem | Difficulty | Link     | Solution                   | Comment |
| --- | ------- | ---------- | -------- | -------------------------- | ------- |
|   208  |    实现 Trie     | 中等       | [题目](https://leetcode-cn.com/problems/implement-trie-prefix-tree/) | [题解]({% link _posts/2021-02-19-implement-trie-prefix-tree.md %}) |   初识前缀树      |
|   648  |    单词替换    | 中等       | [题目](https://leetcode-cn.com/problems/replace-words/) | [题解]({% link _posts/2021-02-19-replace-words.md %}) |   初识前缀树，换汤不换药      |