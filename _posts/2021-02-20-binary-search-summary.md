---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 二分搜索 二分查找 二分法 总结
title: 二分搜索总结
---

> 参考 leetbook

## 一、概述

1. 二分查找是一种「每次都将查找空间一分为二」的算法，因此其时间复杂度为 `O(log n)`

2. 适用于：查找集合中的索引或元素时，都可以考虑二分查找

3. 三个主要部分组成：
   
   1. 预处理： 如果集合未排序，则需先进行排序。
   2. 二分查找： 使用循环或递归在每次比较后将查找空间划分为两半。
   3. 后处理： 在剩余空间中确定可行的候选者。

## 二、模版

## 2.1 模版 I

> 二分查找的最基础和最基本的形式。

1. 模版特点：
   2. 查找条件可以在不与元素的两侧进行比较的情况下确定（或使用它周围的特定元素）。
   3. 不需要后处理：因为每一步中都检查是否找到了元素。如果到达末尾，则知道未找到该元素。

```python
def binarySearch(nums, target):
    if len(nums) == 0:
        return -1
    
    # 初始条件
    left, right = 0, len(nums) - 1
    
    # 终止条件：left > right 时就会跳出循环
    while left <= right:
        mid = (left + right) // 2
        # 检查是否找到元素
        if nums[mid] == target:
            return mid
        # 没找到就移动指针
        # 向右移动
        elif nums[mid] < target:
            left = mid + 1
        # 向左移动
        else:
            right = mid - 1
    
    # 不需要后处理
    return -1
```

## 2.2 模版 II

> 二分查找的高级模板。

1. 模版特点：
   1. 用于查找数组中「当前索引及其直接右邻居索引」的元素或条件。
   2. 查找条件需要访问「元素的直接右邻居」。
   3. 使用「元素的右邻居」来确定它是向左还是向右。
   4. 保证查找空间在每一步中至少有 2 个元素。
   5. 需要进行后处理：当剩下 1 个元素时，循环结束。 需判断其余元素是否符合条件。


```python
def binarySearch(nums, target):
    if len(nums) == 0:
        return -1

    # 初始条件
    left, right = 0, len(nums)

    # 终止条件：left == right 时就会跳出循环
    while left < right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        # 向右移动
        elif nums[mid] < target:
            left = mid + 1
        # 向左移动
        # 右邻居
        else:
            right = mid

    # 需要后处理：此时剩 1 个元素
    # 需判断剩余元素是否符合条件
    if left != len(nums) and nums[left] == target:
        return left
    return -1
```

## 2.3 模版 III

> 也比第一个模版高级一点儿

1. 模版特点：
   1. 用于查找「当前索引及其在数组中的直接左右邻居索引」的元素或条件。
   2. 查找条件需要访问「元素的直接左右邻居」。
   3. 使用「元素的邻居」来确定它是向右还是向左。
   4. 保证查找空间在每个步中至少有 3 个元素。
   5. 需要进行后处理：当剩下 2 个元素时，循环结束。需判断其余元素是否符合条件。

```python
def binarySearch(nums, target):
    if len(nums) == 0:
        return -1

    # 初始条件
    left, right = 0, len(nums) - 1

    # 终止条件：left + 1 == right 时就会跳出循环
    while left + 1 < right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        # 向右移动
        elif nums[mid] < target:
            left = mid
        # 向左移动
        else:
            right = mid

    # 需要后处理：此时剩 2 个元素
    # 需判断剩余元素是否符合条件
    if nums[left] == target: return left
    if nums[right] == target: return right
    return -1
```

## 2.4 模版对比

![binary-search]({{ "./assets/images/binary-search.jpg" | absolute_url }})
 