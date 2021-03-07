---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 双指针 总结
title: 双指针总结
excerpt: 双指针在数组和链表中的常见用法及相关题目总结
---

> 本总结主要是对 LeetCode 官方的 LeetBook 进行笔记的整理和练习题的汇总

# 题型

> 1. 数组中的题型大致分为：一头一尾 & 两个头 
>     - 一头一尾：在序列的首尾各放一个指针，经过一定条件的判断，将其中一个指针向序列中部靠拢，直到找到有效答案。
>     - 两个头：在序列的首部放两个指针，在一定的条件下，两个指针以不同的步长向后移动，直到找到有效答案。
> 
> 2. 当然，链表中也可以使用双指针，常见的是利用快慢指针来解题。

## 1. 一头一尾

1. 初始化一头一尾的指针，然后对序列进行操作（如交换元素等），再向序列的中间靠拢。
2. 图源来自 LeetCode 官方：![two-pointers]({{ "./assets/images/two-pointers.gif" | absolute_url }}){:height="75%" width="75%"}

### 相关题目

   | No  | Problem       | Difficulty | Link                                                                       | Solution                                                                 | Comment                                                                      |
   | --- | ------------- | ---------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
   | 1   | 两数之和      | 简单       | [题目](https://leetcode-cn.com/problems/two-sum/)                          | [题解]({% link _posts/2021-03-04-two-sum.md %})                          | 这道题不用双指针，但是和「167 两数之和 II」相关。                            |
   | 167 | 两数之和 II   | 简单       | [题目](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/) | [题解]({% link _posts/2021-03-04-two-sum-ii-input-array-is-sorted.md %}) | 读懂题意再使用一头一尾双指针就可以解出来。也可以使用哈希表的方法。           |
   | 344 | 反转字符串    | 简单       | [题目](https://leetcode-cn.com/problems/reverse-string/)                   | [题解]({% link _posts/2021-03-03-reverse-string.md %})                   | 看懂上面的动图就会做的一道题                                                 |
   | 541 | 反转字符串 II | 简单       | [题目](https://leetcode-cn.com/problems/reverse-string-ii/)                | [题解]({% link _posts/2021-03-04-reverse-string-2.md %})                 | 和 [344 反转字符串]({% link _posts/2021-03-03-reverse-string.md %}) 相差无几 |
   | 561 | 数组拆分 I    | 简单       | [题目](https://leetcode-cn.com/problems/array-partition-i/)                | [题解]({% link _posts/2021-03-04-array-partition-i.md %})                | 这个双指针不太明显？🥲                                                        |
   

## 2. 两个头

> 给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

1. 思路 1: 初始化一个新的数组，将原始数组中，任何不等于 `val` 的元素都填入新数组中。
2. 思路 2: 要求 **原地移除**
   1. 初始化一个快指针 `fast` 和一个慢指针 `slow`；
   2. `fast`：每次都向前移动一步；
   3. `slow`：只有当 `fast` 不指向 `val` 时才向前移动。
    ![two-pointers-2]({{ "./assets/images/two-pointers-2.gif" | absolute_url }}){:height="75%" width="75%"}

3. 参考代码：
   ```python
    class Solution:
       def removeElement(self, nums: List[int], val: int) -> int:
           slow = 0
           for fast in range(len(nums)):
               if nums[fast] != val:
                   nums[slow] = nums[fast]
                   slow += 1
           return slow
   ```

### 相关题目

   | No  | Problem           | Difficulty | Link                                                                | Solution                                                          | Comment                                                                   |
   | --- | ----------------- | ---------- | ------------------------------------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------- |
   | 27  | 移除元素          | 简单       | [题目](https://leetcode-cn.com/problems/remove-element/)            | [题解]({% link _posts/2021-03-05-remove-element.md %})            | 快慢指针在数组里的使用，看懂上面动图就可以做的题。                        |
   | 209 | 长度最小的子数组  | 中等       | [题目](https://leetcode-cn.com/problems/minimum-size-subarray-sum/) | [题解]({% link _posts/2021-03-05-minimum-size-subarray-sum.md %}) | 既可以用双指针的滑动窗口，又可以用二分法，是一道很好的练习题              |
   | 485 | 最大连续 1 的个数 | 简单       | [题目](https://leetcode-cn.com/problems/max-consecutive-ones/)      | [题解]({% link _posts/2021-03-05-max-consecutive-ones.md %})      | 与 [27 移除元素]({% link _posts/2021-03-05-remove-element.md %}) 相差无几 |

## 3. 快慢指针 （链表）

1. 在链表中，快慢指针的用法通常是：慢指针每次移动一步，快指针每次移动两步。
2. 因此，快慢指针能够在：[判断链表是否有环]({% link _posts/2021-03-06-linked-list-cycle.md %})、删除倒数第 N 个节点之类的题目中大展身手。

### 相关题目

   | No  | Problem  | Difficulty | Link                                                        | Solution                                                  | Comment |
   | --- | -------- | ---------- | ----------------------------------------------------------- | --------------------------------------------------------- | ------- |
   | 141 | 环形链表 | 简单       | [题目](https://leetcode-cn.com/problems/linked-list-cycle/) | [题解]({% link _posts/2021-03-06-linked-list-cycle.md %}) |    快慢指针在链表的经典应用     |
   | 142 | 环形链表 II | 中等       | [题目](https://leetcode-cn.com/problems/linked-list-cycle-ii/) | [题解]({% link _posts/2021-03-06-linked-list-cycle-ii.md %}) |    快慢指针在链表的经典应用，比 [141 环形链表]({% link _posts/2021-03-06-linked-list-cycle.md %}) 更进阶一丢丢～     |
   | 剑指 Offer 22 |  链表中倒数第k个节点 | 简单       | [题目](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/) | [题解]({% link _posts/2021-03-07-nth-node-from-end-of-list.md %}) |    速度相同的快慢指针在链表的经典应用     |
   | 19 |   删除链表的倒数第 N 个结点 | 中等       | [题目](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/) | [题解]({% link _posts/2021-03-07-remove-nth-node-from-end-of-list.md %}) |    速度相同的快慢指针在链表的经典应用，该题解是基于 [链表中倒数第k个节点]({% link _posts/2021-03-07-nth-node-from-end-of-list.md %}) 的解法进阶而来，需要注意链表的一些边界处理～    |
  