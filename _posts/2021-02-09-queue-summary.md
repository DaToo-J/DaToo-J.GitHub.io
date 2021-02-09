---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 队列 总结
title: 队列总结
---


# 分类

## 队列

1. 特点：First In First Out，先入先出。（图来自 LeetCode 官方）
   ![queue]({{ "./assets/images/queue.gif" | absolute_url }}){:height="50%" width="50%"}

2. 操作：
   1. 入队(enqueue) ：也叫插入(insert)，新元素会被添加至队列的末尾；
   
   2. 出队(dequeue) ：也叫删除(delete)，只能移除队首的第一个元素。

## 循环队列

1. （图来自 LeetCode 官方） ![circular-queue]({{ "./assets/images/circular-queue.gif" | absolute_url }}){:height="50%" width="50%"}

2. 实现：利用一个数组和两个指针实现
3. 相关题目：[622 设计循环队列]({% link _posts/2021-02-09-design-circular-queue.md %})        

## 优先队列

# 应用

## BFS

1. 队列可用于 BFS(广度优先搜索)

2. BFS 擅长：从根结点到目标结点的最短路径，当找到最短路径时，可以提前终止。

3. BFS 步骤：

   1. 第一轮：处理根节点

   2. 第二轮：处理根节点的子节点

   3. 第三轮：处理根节点的子节点的子节点

   4. 第 k 轮：处理距离根节点 k-1 步的节点

4. 出队入队：
   1. 出队：处理当前节点时，当前节点将会出队

   2. 入队：处理当前节点时，当前节点的子节点都将入队。新入队的节点不会被马上处理，而是等到它们成为队首元素时才会被处理。

5. 相关题目：套用以下模版即可通过的第 200 题 [岛屿数量]({% link _posts/2021-02-09-number-of-islands.md %})   

6. 模版：

    ```python
    import queue

    visited = []

    def bfs():
        q = queue.Queue()
        q.put(初始状态)
        while q.qsize() > 0:
            i = q.get()
            if visited[i]: continue
            if i 是我们要找的目标: return
            for j in i 可抵达状态:
                if j 合法：
                    q.put(j)
        return 没找到
    ```

## 层次遍历

1. 二叉树的层次遍历

2. 层次遍历：不像 BFS 需要提前终止，是 BFS 的副产物。

3. 参考：Lucifer 文章

4. 模版：

    ```python
    from collections import deque

    dq = deque()
    dq.append(root)
    res = []

    while dq:
        curr = dq.popleft()
        res.append(curr.val)
        if curr.left:
            dq.append(curr.left)
        if curr.right:
            dq.append(curr.right)

    return res
    ```