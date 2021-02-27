---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 数组
title: 56 合并区间
---

## 题目

> 题目链接：[56 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间。

示例 1：

    输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
    输出：[[1,6],[8,10],[15,18]]
    解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].

示例 2：

    输入：intervals = [[1,4],[4,5]]
    输出：[[1,5]]
    解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
 
提示：

    1 <= intervals.length <= 104
    intervals[i].length == 2
    0 <= starti <= endi <= 104




## 思路 

1. 变量：
   
   1. `curStart`：（已遍历）当前区间的开始
   2. `curEnd`：（已遍历）当前区间的结束
   3. `nxtStart`：（正在遍历）下一个区间的开始
   4. `nxtEnd`：（正在遍历）下一个区间的结束

2. 按情况讨论：
   1. 如果当前区间为 `[1,3]`，下一个区间为 `[2,6]`。那么，`curStart=1, curEnd=3, nxtStart=2, nxtEnd=6`，即 `curEnd >= nxtStart`。说明「下一个区间的开始」覆盖了「当前区间的结束」，此时需要向右扩展区间长度，更新「当前区间的结束」 `curEnd`。
   2. 如果当前区间为 `[2,6]`，下一个区间为 `[8,10]`。那么，`curStart=2, curEnd=6, nxtStart=8, nxtEnd=10`，即 `curEnd < nxtStart`。说明「当前区间的结束」达不到「下一个区间的开始」，此时需要将「当前区间」添入至结果数组中，并重新更新 `curStart, curEnd`。


## 代码 

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        # 要先排序
        intervals = sorted(intervals, key=lambda k: k[0])

        # 初始化：curStart, curEnd
        curStart, curEnd = intervals[0]
        res = []

        for i in range(1, len(intervals)):
            nxtStart, nxtEnd = intervals[i]
            # 情况一
            if curEnd >= nxtStart:
                curEnd = max(curEnd, nxtEnd)
            # 情况二
            else:
                # 添加至结果数组
                res.append([curStart, curEnd])
                # 更新 curStart, curEnd
                curStart, curEnd = nxtStart, nxtEnd
        
        # 别漏掉最后一个区间
        res.append([curStart, curEnd])
        return res
         
```

## 分析 

1. 时间复杂度和空间复杂度都需要 `O(n)`