---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: DFS 深度优先 图 队列 BFS 广度优先
title: 841 钥匙和房间
excerpt: 一道有多个解法的搜索题目
---

## 题目

> 题目链接：[841 钥匙和房间](https://leetcode-cn.com/problems/keys-and-rooms/)

有 N 个房间，开始时你位于 0 号房间。每个房间有不同的号码：0，1，2，...，N-1，并且房间里可能有一些钥匙能使你进入下一个房间。

在形式上，对于每个房间 i 都有一个钥匙列表 rooms[i]，每个钥匙 rooms[i][j] 由 [0,1，...，N-1] 中的一个整数表示，其中 N = rooms.length。 钥匙 rooms[i][j] = v 可以打开编号为 v 的房间。

最初，除 0 号房间外的其余所有房间都被锁住。

你可以自由地在房间之间来回走动。

如果能进入每个房间返回 true，否则返回 false。

示例 1：

    输入: [[1],[2],[3],[]]
    输出: true
    解释:  
        我们从 0 号房间开始，拿到钥匙 1。
        之后我们去 1 号房间，拿到钥匙 2。
        然后我们去 2 号房间，拿到钥匙 3。
        最后我们去了 3 号房间。
        由于我们能够进入每个房间，我们返回 true。
示例 2：

    输入：[[1,3],[3,0,1],[2],[0]]
    输出：false
    解释：我们不能进入 2 号房间。
    提示：
        1 <= rooms.length <= 1000
        0 <= rooms[i].length <= 1000
        所有房间中的钥匙数量总计不超过 3000。

## 思路 

1. 写一个递归函数：输入房间号，然后用这个房间里的钥匙去开别的房间。

    1. 搜索空间：该房间里的钥匙，`for r in rooms[room]`

    2. 限制：如果有房间被解锁过，那就无需再次解锁，否则就陷入死循环。`if r not in self.visited`

    3. 由于每次都访问尚未遍历的房间，因此，访问新房间时，可以进行计数，以便递归结束后和总房间数做比较。

## 代码 

### 递归的解法

> DFS

```python
class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        '''
        递归的形式
        '''
        def dfs(room):
            # 将遍历过的房间计入 visited
            self.visited.add(room)
            self.count += 1

            for r in rooms[room]:
                # 每次访问的都是尚未被遍历的房间
                # 保证 self.count 统计的都是唯一的
                if r not in self.visited:
                    dfs(r)
        
        self.visited = set()
        self.count = 0
        dfs(0)
        return self.count == len(rooms)
```

### 循环的解法

> BFS

```python
class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        '''
        改写成队列的形式
        '''
        import queue
        q = queue.Queue()
        q.put(0)
        self.count = 0
        self.visited = {0}

        while q.qsize() > 0:
            cur = q.get()
            self.count += 1
            for r in rooms[cur]:
                if r not in self.visited:
                    q.put(r)
                    self.visited.add(r)
                
        return self.count == len(rooms)
```

## 分析 

1. 空间复杂度需要 `O(n)`, `n` 为房间数；
2. 时间复杂度需要 `O(n+m)`, `n` 为房间数， `m` 为房间里的钥匙数。