---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 并查集 总结
title: 并查集总结
excerpt: 并查集的基本思路、常见题目汇总
---

## 一、介绍

### 1. 是什么

1. 英文：Disjoint-set data structure 或 Union-find data structure
2. 直译：「没有交集的数据结构」或「联合-查找数据结构」
3. 本质上，是一种图结构。这种图的特点可以用 “不交集森林(Disjoin-set forest)” 概括：将集合当作是一棵树，在森林中，有许多这样的树（集合）并且它们之间没有交集。
4. 表示：
   1. 每个集合用一棵树表示，每个元素用每个节点表示；
   2. 树的每个节点（图中橙色节点）都保存着它的父节点；
   3. 树的根节点（图中绿色节点）则保存一个空引用或者到自身的引用或者其他无效值，以表示自身为根节点。

![union-find-1]({{ "./assets/images/union-find-1.jpeg" | absolute_url }})

### 2. 可以做什么

> 支持一系列没有重复元素的集合的 **查询和合并** 操作

1. 查询：返回待查询元素所属的 **集合的代表元素**。主要用于判断两个元素是否同属于一个集合。如上图：
   1. 查找节点 `D`，返回其所在树（集合）的代表元素 `A`；
   1. 查找节点 `G`，返回其所在树（集合）的代表元素 `I`；
   1. 查找节点 `B`，返回其所在树（集合）的代表元素 `A`；
   2. 节点 `D` 和 `B` 因为代表元素相同，则同属于一颗树；而节点 `G` 在另一个棵树里。

2. 合并：将两个集合合并为一个集合。

### 3. 需要注意的

![union-find-2]({{ "./assets/images/union-find-2.jpeg" | absolute_url }})

1. 如图(a)：当在集合很大或者树很不平衡时，树就变成了「一颗线性树」，单次查询的时间复杂度高达 ${\displaystyle \mathrm {O} \left(n\right)}$。

2. 如图(b)：一个常见的优化是路径压缩：在查询时，把被查询的节点到根节点的路径上的所有节点的父节点设置为根结点，从而减小树的高度。也就是说，在向上查询的同时，把在路径上的每个节点都直接连接到根上，以后查询时就能直接查询到根节点。

## 二、模版


## 三、相关题目