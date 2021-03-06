---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 二分搜索 总结
title: 二分搜索总结
excerpt: 二分搜索的基本概念、使用场景、模版使用、以及相关题目的总结
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

### 2.1 模版 I

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

### 2.2 模版 II

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

### 2.3 模版 III

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

### 2.4 模版对比

![binary-search]({{ "./assets/images/binary-search.jpg" | absolute_url }})
 
## 三、相关模块

> python 模块 [`bisect`（数组二分查找算法）](https://docs.python.org/zh-cn/3.6/library/bisect.html) 可用于 **维护有序列表**。`bisection` 是一分为二的意思。
> 
> `bisect` 的操作是基于二分搜索来实现的，相比于循环和递归的二分性能更好一些。
> 
> `bisect` 主要操作可分为两类： `bisect*`（搜索）和 `insort*`（插入）


1. `bisect*`（搜索）：
   1. `bisect.bisect_left(nums, item, lo=0, hi=len(nums))`：
      1. `nums`：待搜索的 **有序列表**
      2. `item`：待搜索的元素
      3. 函数作用：如果要在有序列表 `nums` 中插入元素 `item`，应该插入到哪个位置，该函数会返回这个位置索引。如果 `nums` 中存在 `item`，则返回其左边的索引。
      4. `lo, hi`：指定有序列表的搜索区间，默认是整个列表。
    2. `bisect.bisect_right(nums, item, lo=0, hi=len(nums))`：和 `bisect_left()` 相似，如果 `item` 已存在，则返回其右边的索引。
    3. `bisect.bisect(nums, item, lo=0, hi=len(nums))`：和 `bisect_left()` 相似，如果 `item` 已存在，则返回其右边的索引。

2. `insort*`（插入）：
   1. `bisect.insort_left(nums, item, lo=0, hi=len(nums))`：`bisect_left()` 二分搜索后，再插入。`nums.insert(bisect.bisect_left(nums, item, lo, hi), item)` 的效果相同。
   2. `bisect.insort_right(nums, item, lo=0, hi=len(nums))`：同 `nums.insert(bisect.bisect_right(nums, item, lo, hi), item)`
   3. `bisect.insort(nums, item, lo=0, hi=len(nums))`：同 `nums.insert(bisect.bisect(nums, item, lo, hi), item)`

3. 🌰：
    ```python
    import bisect
    import random

    nums = []
    print("item idx  nums")
    print("---- ---  ----")
    for _ in range(10):
        item = random.randint(1, 100)

        # 通过二分搜索获取 item 插入到 nums 的索引
        idx = bisect.bisect_left(nums, item)
        # 插入 item
        bisect.insort_left(nums, item)

        print("%4d %3d " % (item, idx), nums)
    ```
    结果输出：
    ```  
    item idx  nums
    ---- ---  ----
    42   0  [42]
    24   0  [24, 42]
    42   1  [24, 42, 42]
    75   3  [24, 42, 42, 75]
    59   3  [24, 42, 42, 59, 75]
    68   4  [24, 42, 42, 59, 68, 75]
    31   1  [24, 31, 42, 42, 59, 68, 75]
    96   7  [24, 31, 42, 42, 59, 68, 75, 96]
    57   4  [24, 31, 42, 42, 57, 59, 68, 75, 96]
    91   8  [24, 31, 42, 42, 57, 59, 68, 75, 91, 96]
    ```

4. 还可用于分数等级的计算：
    ```python
    def getGrade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
        i = bisect.bisect(breakpoints, score)
        return grades[i]

    [getGrade(score) for score in [24, 31, 42, 42, 57, 59, 68, 75, 91, 96]]
    # ['F', 'F', 'F', 'F', 'F', 'F', 'D', 'C', 'A', 'A']
    ```
5. 在 209 题 [长度最小的子数组]({% link _posts/2021-03-05-minimum-size-subarray-sum.md %}) 中有使用到 `bisect` 模块。



## 四、相关题目



   | No  | Problem                                    | Difficulty | Link                                                                                              | Solution                                                                                        | Comment                                                                      |
   | --- | ------------------------------------------ | ---------- | ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
   | 69  | x 的平方根                                 | 简单       | [题目](https://leetcode-cn.com/problems/sqrtx/)                                                   | [题解]({% link _posts/2021-02-23-sqrt-x.md %})                                                  | 二分搜索的入门题目，使用「模版 I」即可                                       |
   | 33  | 搜索旋转排序数组                           | 中等       | [题目](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)                          | [题解]({% link _posts/2021-02-23-search-in-rotated-sorted-array.md %})                          | 比「x 的平方根」更进阶一点，二分搜索的另一种使用场景。同样使用「模版 I」即可 |
   | 209 | 长度最小的子数组 | 中等       | [题目](https://leetcode-cn.com/problems/minimum-size-subarray-sum/) | [题解]({% link _posts/2021-03-05-minimum-size-subarray-sum.md %}) | 既可以用双指针的滑动窗口，又可以用二分法，是一道很好的练习题。使用到了 `bisect` 模块。 |
   | 278 | 第一个错误的版本                           | 简单       | [题目](https://leetcode-cn.com/problems/first-bad-version/)                                       | [题解]({% link _posts/2021-02-24-first-bad-version.md %})                                       | 二分搜索「模版 II」的入门题目                                                |
   | 162 | 寻找峰值                                   | 中等       | [题目](https://leetcode-cn.com/problems/find-peak-element/)                                       | [题解]({% link _posts/2021-02-24-find-peak-element.md %})                                       | 二分搜索「模版 II」的进阶题目。二分搜索不仅仅局限于左右指针与中间值的比较。  |
   | 34  | 在排序数组中查找元素的第一个和最后一个位置 | 中等       | [题目](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | [题解]({% link _posts/2021-02-25-find-first-and-last-position-of-element-in-sorted-array.md %}) | 二分搜索「模版 III」和「模版 I」都可以使用的进阶题目。不同的解题思路使用不同的模版。                                             |
   | 658  | 找到 K 个最接近的元素 | 中等       | [题目](https://leetcode-cn.com/problems/find-k-closest-elements/) | [题解]({% link _posts/2021-02-25-find-k-closest-elements.md %}) | 用到了「模版 I」，但又不局限于[二分搜索总结]({% link _posts/2021-02-20-binary-search-summary.md %}) 里的模版。在搜索时还需要根据解题思路魔改模版。                                             |