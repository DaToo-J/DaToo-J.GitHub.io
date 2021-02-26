---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags:  数组 栈
title: 768 最多能完成排序的块 II
---

## 题目

> 题目链接：[最多能完成排序的块 II](https://leetcode-cn.com/problems/max-chunks-to-make-sorted-ii/)

这个问题和“最多能完成排序的块”相似，但给定数组中的元素可以重复，输入数组最大长度为2000，其中的元素最大为10**8。

arr是一个可能包含重复元素的整数数组，我们将这个数组分割成几个“块”，并将这些块分别进行排序。之后再连接起来，使得连接的结果和按升序排序后的原数组相同。

我们最多能将数组分成多少块？

示例 1:

    输入: arr = [5,4,3,2,1]
    输出: 1
    解释:
        将数组分成2块或者更多块，都无法得到所需的结果。
        例如，分成 [5, 4], [3, 2, 1] 的结果是 [4, 5, 1, 2, 3]，这不是有序的数组。 

示例 2:

    输入: arr = [2,1,3,4,4]
    输出: 4
    解释:
        我们可以把它分成两块，例如 [2, 1], [3, 4, 4]。
        然而，分成 [2, 1], [3], [4], [4] 可以得到最多的块数。 


## 思路

### 1. 块的特点

1. 当前块的最大值 < 下一块的所有元素；

2. 如果当前元素 > 之前的元素，则当前元素很可能是新的块；如果下一个元素 < 之前元素，说明这个块还没结束，下一个元素和之前元素是同一个块，这意味着涉及到回退。
   
### 2. 步骤

1. 借鉴 [Krahets](https://leetcode-cn.com/problems/max-chunks-to-make-sorted-ii/solution/zui-duo-neng-wan-cheng-pai-xu-de-kuai-ii-deng-jie-/) 题解

2. 记当前块的最大值为 `head`，将各个块的 `head` 都存入栈。

3. 如果 `当前元素 > stack[-1]`，说明可以开辟一个新的块，则直接 `append` 新的块的 `head` ，即 当前元素；

4. 如果 `当前元素 < stack[-1]`，说明当前元素 至少 是 `stack[-1]` 这个块里的，所以要 `pop` 出`stack[-1]`. 至于是不是 `stack[-2],stack[-3],...` 块内的，也要一一比较，直到当前元素被 `stack[-1]` 所在的块包含进去。


## 代码

```python
class Solution:
    def maxChunksToSorted(self, arr: List[int]) -> int:
        stack = []
        for n in arr:
            if stack and stack[-1] > n:
                head = stack.pop()
                while stack and stack[-1] > n:
                    stack.pop()
                stack.append(head)
            else:
                stack.append(n)
        return len(stack)
```

## 分析

1. 时间复杂度需要 `O(n * n)`：遍历一次需要 `O(n)`，修正 `pop` 时最多需要 `O(n)`。
2. 空间复杂度需要 `O(n)`：最坏的情况下一共有 `n` 个块。