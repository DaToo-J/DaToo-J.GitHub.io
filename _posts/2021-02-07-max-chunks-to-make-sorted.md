---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 基础 数组 栈
title: 
---

## 思路

> 题目链接：[最多能完成排序的块 II](https://leetcode-cn.com/problems/max-chunks-to-make-sorted-ii/)

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