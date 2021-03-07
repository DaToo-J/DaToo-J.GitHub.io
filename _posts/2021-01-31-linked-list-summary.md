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



### 相关题目

   | No  | Problem  | Difficulty | Link                                                        | Solution                                                  | Comment |
   | --- | -------- | ---------- | ----------------------------------------------------------- | --------------------------------------------------------- | ------- |
   | 141 | 环形链表 | 简单       | [题目](https://leetcode-cn.com/problems/linked-list-cycle/) | [题解]({% link _posts/2021-03-06-linked-list-cycle.md %}) |    快慢指针在链表的经典应用     |
   | 142 | 环形链表 II | 中等       | [题目](https://leetcode-cn.com/problems/linked-list-cycle-ii/) | [题解]({% link _posts/2021-03-06-linked-list-cycle-ii.md %}) |    快慢指针在链表的经典应用，比 [141 环形链表]({% link _posts/2021-03-06-linked-list-cycle.md %}) 更进阶一丢丢～     |
   | 剑指 Offer 22 |  链表中倒数第k个节点 | 简单       | [题目](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/) | [题解]({% link _posts/2021-03-07-nth-node-from-end-of-list.md %}) |    速度相同的快慢指针在链表的经典应用     |
   | 19 |   删除链表的倒数第 N 个结点 | 中等       | [题目](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/) | [题解]({% link _posts/2021-03-07-remove-nth-node-from-end-of-list.md %}) |    速度相同的快慢指针在链表的经典应用，该题解是基于 [链表中倒数第k个节点]({% link _posts/2021-03-07-nth-node-from-end-of-list.md %}) 的解法进阶而来，需要注意链表的一些边界处理～    |
  