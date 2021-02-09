---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 队列 循环队列 设计
title: 622 设计循环队列
---

## 题目

> 题目链接：[622 设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)

设计你的循环队列实现。 循环队列是一种线性数据结构，其操作表现基于 FIFO（先进先出）原则并且队尾被连接在队首之后以形成一个循环。它也被称为“环形缓冲器”。

循环队列的一个好处是我们可以利用这个队列之前用过的空间。在一个普通队列里，一旦一个队列满了，我们就不能插入下一个元素，即使在队列前面仍有空间。但是使用循环队列，我们能使用这些空间去存储新的值。

你的实现应该支持如下操作：

    MyCircularQueue(k): 构造器，设置队列长度为 k 。
    Front: 从队首获取元素。如果队列为空，返回 -1 。
    Rear: 获取队尾元素。如果队列为空，返回 -1 。
    enQueue(value): 向循环队列插入一个元素。如果成功插入则返回真。
    deQueue(): 从循环队列中删除一个元素。如果成功删除则返回真。
    isEmpty(): 检查循环队列是否为空。
    isFull(): 检查循环队列是否已满。
 

示例：

    MyCircularQueue circularQueue = new MyCircularQueue(3); // 设置长度为 3
    circularQueue.enQueue(1);  // 返回 true
    circularQueue.enQueue(2);  // 返回 true
    circularQueue.enQueue(3);  // 返回 true
    circularQueue.enQueue(4);  // 返回 false，队列已满
    circularQueue.Rear();  // 返回 3
    circularQueue.isFull();  // 返回 true
    circularQueue.deQueue();  // 返回 true
    circularQueue.enQueue(4);  // 返回 true
    circularQueue.Rear();  // 返回 4
 

提示：

    所有的值都在 0 至 1000 的范围内；
    操作数将在 1 至 1000 的范围内；
    请不要使用内置的队列库。


## 思路 I 

1. 常规的思路：用数组实现

2. 注意：每次出队、入队、查询队首、查询队尾时，都需要判断队列是否为空、已满。

## 代码 I

```python
class MyCircularQueue:

    def __init__(self, k: int):
        self.length = k
        self.queue = []

    def enQueue(self, value: int) -> bool:
        if len(self.queue) >= self.length:
            return False
        self.queue.append(value)
        return True

    def deQueue(self) -> bool:
        if len(self.queue) == 0: return False
        self.queue.pop(0)
        return True

    def Front(self) -> int:
        if len(self.queue) == 0: return -1
        return self.queue[0]

    def Rear(self) -> int:
        if len(self.queue) == 0: return -1
        return self.queue[-1]

    def isEmpty(self) -> bool:
        return len(self.queue) == 0

    def isFull(self) -> bool:
        return len(self.queue) == self.length

```



## 思路 II 

1. 指针的思路：

   1. `start`：指向队首元素

   2. `end`：指向队尾元素的后一个元素

2. 操作：入队 & 出队
   1. 需判断队列是否已满 or 为空。
   2. 入队：因为 `end` 指向队尾元素的后一个元素，此时 `queue[end]` 为空，把新元素的值赋给 `queue[end]`。然后，指针 `end` 后移一位。
   3. 出队：将队首元素置空，即 `queue[start] = -1`。然后，指针 `start` 后移一位。

3. 查询：队首 & 队尾
   1. 需判断队列是否已满 or 为空。
   2. 队首：即 `queue[start]`
   3. 队尾：即 `queue[end-1]`

4. 判断队列是否已满 or 为空：根据题意可知，所有的值都大于等于 `0`，所以初始化的时候可给数组元素赋值 `-1`。因此，队列的状态（已满 or 为空）可根据数组中值为 `-1` 的个数来判断。


## 代码 II

```python
class MyCircularQueue:

    def __init__(self, k: int):
        self.length = k
        self.queue = [-1] * self.length
        self.start, self.end = 0, 0


    def enQueue(self, value: int) -> bool:
        if self.isFull(): return False

        self.queue[self.end] = value
        if self.end == self.length - 1:
            self.end = 0
        else:
            self.end += 1

        return True


    def deQueue(self) -> bool:
        if self.isEmpty(): return False

        self.queue[self.start] = -1
        if self.start == self.length - 1:
            self.start = 0
        else:
            self.start += 1

        return True


    def Front(self) -> int:
        return self.queue[self.start]


    def Rear(self) -> int:
        return self.queue[self.end-1]


    def isEmpty(self) -> bool:
        return self.queue.count(-1) == self.length


    def isFull(self) -> bool:
        return self.queue.count(-1) == 0

```


