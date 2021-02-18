---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 哈希表 总结
title: 哈希表总结
---

# 一、常见问题

> 哈希表是一种数据结构，使用哈希函数组织数据，支持 **快速插入和搜索**

## 1. 哈希表原理

1. 关键思想：使用哈希函数将「键」映射到「存储桶」

2. 插入：哈希函数决定将键分配到哪个桶，并将该键存储在相应的桶中

3. 搜索：使用相同的哈希函数来找到对应的桶，并在桶内进行搜索

## 2. 哪些因素会影响哈希函数以及冲突解决策略的选择？

### 2.1 哈希函数

1. 哈希函数是哈希表的重要组件：是将给定数据转化为固定长度的不规则值的函数

2. 特点：
   1. 是一个数字，通常用 16 进制表示
   2. 输出数据的长度取决于哈希函数。不同的哈希函数，输出的数据长度可能不一样。当同样的哈希函数，输出的数据长度一定一样。
   3. 使用同一个哈希函数，输入相同，输出的数据也相同。
   4. 使用同一个哈希函数，即使输入相似，输出的数据也不同。
   5. 即使输入完全不同的数据，输出相同的哈希值会以很低的概率出现。这就是“哈希值的冲突”。
   6. 不能通过哈希值倒推出输入的数据。
3. 将「键」映射到「存储桶」

4. 散列函数：取决于「键值范围」和「桶的数量」。如：`y = x % 9` 就可以作为散列函数， `x` 是键值，`y` 是分配桶的索引。

5. 主要思想：尽可能将键分配到不同的桶中。完美的情况下，键和桶是一对一映射。但大多数情况下，键值范围和桶的数量之间有冲突，需要在二者之间进行权衡。

### 2.2 冲突解决

1. 冲突解决算法应该解决以下几个问题：
   1. 如何组织在同一个桶中的值？
   2. 如果为同一个桶分配了太多的值，该怎么办？
   3. 如何在特定的桶中搜索目标值？


## 3. 哈希集合与哈希映射之间的区别。

### 3.1 哈希集合

1. 是一种存储不重复值的数据结构。在 python 中可用 `set()` 实现。

2. 操作：插入、删除、搜索

3. 应用：用于查重

### 3.2 哈希映射

1. 是映射的一种实现。在 python 中用 `dict()` 实现。

2. 能将 `(key, value)` 键值对存储下来

## 4. 如何设计如典型的标准模板库中那样的哈希集合和哈希映射的简化版本。

## 5. 插入和查找操作的复杂度是什么？

1. 哈希表的典型设计：

   1. 键值可以是任何 **可哈希化的** 类型
   2. 每个桶包含一个数组，初始化时将所有值存在同一个桶
   3. 如果同一个桶有太多的值，这些值将被保留在一个 **高度平衡的二叉搜索树** 中。

2. 理想情况下，桶的大小足够小：插入和搜索的时间复杂度都是 `O(1)`

3. 最坏情况下：桶的最大值为 `N`
   1. 如果桶使用的是数组来实现，插入的时间复杂度为 `O(1)`，搜索的时间复杂度为 `O(N)`。
   2. 如果桶使用的是高度平衡的二叉搜索树，插入和搜索的时间复杂度都为 `O(log N)`。


> 参考：LeetCode 官方



---

# 二、使用

## `dict()`

> python 原生的 `dict`

```python
# 初始化一个字典（哈希表）
d = {}
# 或
d = dict()

 
# 删除键是'Name'的条目
del d['Name']  

# 清空字典所有条目
d.clear()      

# 删除字典
del d          

# 查找 key 对应的值，没有则返回 default 
d.get(key, default=None)

# 哈希表中是否有 key 这个键
d.has_key(key)


# 和get()类似, 但如果键不存在于字典中，将会添加键并将值设为 default
d.setdefault(key, default=None)
```

## `defaultdict()`

> `collections` 类中的 `defaultdict()`方法可以为字典提供默认值

1. 语法：`collections.defaultdict([default_factory[, …]])`
2. 字典的 `values` 是 `function_factory` 的类实例，而且具有默认值。比如：`default(int)` 则创建一个类似 dictionary 对象，里面任何的 `values` 都是 `int` 的实例。如果是一个不存在的 `key`, `d[key]` 也有一个默认值，这个默认值是 `int()` 的默认值 `0`.
3. `defaultdict()` 的作用等于 `dict + setdefault`

```python
from collections import defaultdict

# 类型为 int
# 可用作统计频率
dd = defaultdict(int)
s = 'mississippi'
for k in s:
    dd[k] += 1
# >>> defaultdict(int, {'m': 1, 'i': 4, 's': 4, 'p': 2})


# 类型为 list
dd = defaultdict(list)
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
for k, v in s:
    dd[k].append(v)
# >>> defaultdict(list, {'yellow': [1, 3], 'blue': [2, 4], 'red': [1]})


# Use dict and setdefault    
g = {}
for k, v in s:
    g.setdefault(k, []).append(v)
```

## `Counter`

> 计数器

```python
from collections import Counter

temp = Counter('abcdeabcdabcaba')
# >>> Counter({'a': 5, 'b': 4, 'c': 3, 'd': 2, 'e': 1})

# 前 n 个出现最多次数的元素
temp.most_common(1) # >>> [('a', 5)]
temp.most_common(2) # >>> [('a', 5), ('b', 4)]
temp.most_common() # >>> [('a', 5), ('b', 4), ('c', 3), ('d', 2), ('e', 1)]

```


# 三、相关题目



| No  | Problem          | Difficulty | Link                                                                    | Solution                                                              | Comment            |
| --- | ---------------- | ---------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------- | ------------------ |
| 16.02  | 单词频率 | 中等       | [题目](https://leetcode-cn.com/problems/words-frequency-lcci/) | [题解]({% link _posts/2021-02-18-words-frequency-lcci.md %}) | 哈希表的基本使用 |
| 1261  | 在受污染的二叉树中查找元素 | 中等       | [题目](https://leetcode-cn.com/problems/find-elements-in-a-contaminated-binary-tree/) | [题解]({% link _posts/2021-02-18-find-elements-in-a-contaminated-binary-tree.md %}) | 哈希表的使用 |