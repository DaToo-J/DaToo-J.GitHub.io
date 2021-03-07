---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: 总结 链表
title: 链表总结
excerpt: 链表题目的解题思路汇总，不断更新中 ...
---

# 总结

## 1. 虚拟节点

1. 方案一：如果需要一个指针记录更新后的链表，但又不想使用 `head`  ，可以设置一个虚拟节点 `dummy`  ，虚拟节点的下一个节点指向最终的结果链表，此后遍历更新结果时使用 `.next` 来更新。

     1. `dummy` ：指向结果链表的虚拟节点，`dummy.next` 即为结果链表；

     2. `curr` ：用于遍历节点的指针；

     3. 相关的题：[86. 分隔链表](https://leetcode-cn.com/problems/partition-list/) [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/) [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

        ```python
        # 方案一
        dummy = ListNode(-1)
        curr = dummy         # 这样才可以被复制，指向同一个引用地址

        while 某个条件:
          curr.next = 更新结果链表的节点
          curr = curr.next
        return dummy.next
        ```

2. 方案二：但有的时候不一定要给 `dummy` 节点赋值，直接 `prev = None` ，通过遍历来赋值。
     
    1. 相关的题：[206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

        ```python
         # 方案二
         prev, curr = None, head
         while 某个条件：
             prev = XX
         ```

## 2. 双指针

1. 在链表题中，双指针是很常用的技巧。通常，这两个指针都是从链表的头部出发。至于指针在什么时候移动，是否有预处理，以什么速度移动，则需要根据题目的特点来分析。

2. 链表中的双指针可以有不同的走法，如：
   1. 一个指针每次移动一步，另一个指针每次移动两步。
   2. 一个指针先移动 n 步，然后两个指针同时以每次一步的速度移动。

3. 针对这两种的走法有这些题目：
   1. 不同速度的双指针：[判断链表是否有环]({% link _posts/2021-03-06-linked-list-cycle.md %})
   2. 相同速度不同起点的双指针：[倒数第N个节点]({% link _posts/2021-03-07-nth-node-from-end-of-list.md %})、[删除链表的倒数第N个结点]({% link _posts/2021-03-07-remove-nth-node-from-end-of-list.md %}) 、[找到相交链表的交点]({% link _posts/2021-03-07-intersection-of-two-linked-lists.md %})
   3. 以及两种方式结合的： [找到环形链表的入口节点]({% link _posts/2021-03-06-linked-list-cycle-ii.md %})

### 相关题目

   | No            | Problem                   | Difficulty | Link                                                                                   | Solution                                                                 | Comment                                                                                                                                                                        |
   | ------------- | ------------------------- | ---------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
   | 141           | 环形链表                  | 简单       | [题目](https://leetcode-cn.com/problems/linked-list-cycle/)                            | [题解]({% link _posts/2021-03-06-linked-list-cycle.md %})                | 快慢指针在链表的经典应用                                                                                                                                                       |
   | 142           | 环形链表 II               | 中等       | [题目](https://leetcode-cn.com/problems/linked-list-cycle-ii/)                         | [题解]({% link _posts/2021-03-06-linked-list-cycle-ii.md %})             | 快慢指针在链表的经典应用，比 [141 环形链表]({% link _posts/2021-03-06-linked-list-cycle.md %}) 更进阶一丢丢～                                                                  |
   | 160           | 相交链表                  | 简单       | [题目](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)             | [题解]({% link _posts/2021-03-07-intersection-of-two-linked-lists.md %}) | 即可以用栈来实现，也可以用双指针来实现。双指针是如何对算法的空间复杂度进行优化的，这道题是个很好的例子。                                                                       |
   | 剑指 Offer 22 | 链表中倒数第k个节点       | 简单       | [题目](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/) | [题解]({% link _posts/2021-03-07-nth-node-from-end-of-list.md %})        | 速度相同的快慢指针在链表的经典应用                                                                                                                                             |
   | 19            | 删除链表的倒数第 N 个结点 | 中等       | [题目](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)             | [题解]({% link _posts/2021-03-07-remove-nth-node-from-end-of-list.md %}) | 速度相同的快慢指针在链表的经典应用，该题解是基于 [链表中倒数第k个节点]({% link _posts/2021-03-07-nth-node-from-end-of-list.md %}) 的解法进阶而来，需要注意链表的一些边界处理～ |
  

## 3. 需注意的点

> 参考 LeetCode 官方的 LeetBook

1. 检查空节点：在使用 `next` 属性时，需判断前置节点是否为空。
   1. 使用 `fast.next` 时：`fast` 不能为空；
   2. 使用 `fast.next.next` 时：`fast` 和 `fast.next` 不能为空。

2. 如果可以，应当检查循环的结束条件。

3. 复杂度分析：
   1. 如果只使用指针，而不使用任何其他额外的空间，那么空间复杂度将是 `O(1)`。
   2. 如果使用快慢指针：每次移动较快的指针 2 步，每次移动较慢的指针 1 步
      1. 如果没有循环，快指针需要 `N/2` 次才能到达链表的末尾，其中 `N` 是链表的长度。
      2. 如果存在循环，则快指针需要 `M` 次才能赶上慢指针，其中 `M` 是链表中环的长度。
      3. 显然，`M <= N`。所以我们将循环运行 N 次。对于每次循环，我们只需要常量级的时间。因此，该算法的时间复杂度总共为 `O(N)`。
