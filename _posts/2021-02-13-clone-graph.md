---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: DFS BFS 递归 队列
title: 133 克隆图
---

## 题目

> 题目链接：[133 克隆图](https://leetcode-cn.com/problems/clone-graph/)

给你无向 连通 图中一个节点的引用，请你返回该图的 深拷贝（克隆）。

图中的每个节点都包含它的值 val（int） 和其邻居的列表（list[Node]）。

    class Node {
        public int val;
        public List<Node> neighbors;
    }
  

测试用例格式：

简单起见，每个节点的值都和它的索引相同。例如，第一个节点值为 1（val = 1），第二个节点值为 2（val = 2），以此类推。该图在测试用例中使用邻接列表表示。

邻接列表 是用于表示有限图的无序列表的集合。每个列表都描述了图中节点的邻居集。

给定节点将始终是图中的第一个节点（值为 1）。你必须将 给定节点的拷贝 作为对克隆图的引用返回。

示例 1：

    输入：adjList = [[2,4],[1,3],[2,4],[1,3]]
    输出：[[2,4],[1,3],[2,4],[1,3]]
    解释：
        图中有 4 个节点。
        节点 1 的值是 1，它有两个邻居：节点 2 和 4 。
        节点 2 的值是 2，它有两个邻居：节点 1 和 3 。
        节点 3 的值是 3，它有两个邻居：节点 2 和 4 。
        节点 4 的值是 4，它有两个邻居：节点 1 和 3 。




## 思路 —— DFS

1. 在 [栈总结]({% link _posts/2021-02-09-stack-summary.md %}) 里有总结 DFS 的递归模版：

    ```python
    def dfs(cur, target, visited):
        if cur is target:
            return True

        for next in neighbors of cur:
            if next not in visited:
                visited.add(next)
                return True if dfs(next, target, visited)
        return False
    ```

2. 不同于其他图遍历，本题还需要将图中的节点及其邻节点也克隆。

3. 采用哈希表 `visited` 保存：`{原图的节点 : 克隆后的节点}`，防止重复遍历陷入死循环。

4. 写一个递归函数：克隆输入的节点
   1. 如果该节点已存在于 `visited`，说明该节点已经被遍历过，且已克隆好，可直接返回。
   2. 如果不存在，则需进行以下步骤来克隆：
      1. 实例化一个和输入节点值相同的对象
      2. 将该对象在 `visited` 中保存
      3. 遍历输入节点的邻节点，将这些邻节点的副本赋值给实例化的对象的邻居。

5. 在连续调用递归函数的时候实现了向图的最深方向遍历，即 DFS。

## 代码 

```python
class Solution:
    def __init__(self):
        self.visited = dict()

    def cloneGraph(self, node: 'Node') -> 'Node':
        if not node: return node
        
        # 1. 已存在于 visited，直接返回
        if node in self.visited:
            return self.visited[node]

        # 2. 不存在，需继续克隆
        # 2.1 实例化一个和输入节点值相同的对象
        resNode = Node(node.val, [])

        # 2.2 将该对象在 `visited` 中保存
        self.visited[node] = resNode

        # 2.3 遍历输入节点的邻节点，将这些邻节点的副本赋值给实例化的对象的邻居。
        if node.neighbors:
            resNode.neighbors = [self.cloneGraph(n) for n in node.neighbors]
        return resNode
```

## 思路 —— BFS

1. 在 [队列总结]({% link _posts/2021-02-09-queue-summary.md %}) 里有总结 BFS 的队列实现的模版：

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
2. 依然采用哈希表 `visited` 保存：`{原图的节点 : 克隆后的节点}`，防止重复遍历陷入死循环。

3. BFS 实现步骤：
   1. 初始化：将第一个节点添入队列中
   2. 从队列中取出一个节点 `cur` ，遍历该节点的邻居们：
      1. 如果邻居未被遍历过，即在 `visited` 中不存在，那么在 `visited` 中初始化该邻居节点。
      2. 如果被遍历过，则从 `visited` 中取出该邻居节点的克隆，即 `visited[nei]`。将其添加至 `cur` 的邻居节点中。需注意的是，添加邻居节点时，应将 **克隆后的邻居节点** 添加至 **克隆后的当前节点** 的邻居中，即 `visited[cur].neighbors`



## 代码 

```python
class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        if not node: return node
        import queue

        # 1. 初始化
        q = queue.Queue()
        q.put(node)
        
        visited = {}
        visited[node] = Node(node.val, [])

        while q.qsize() > 0:
            # 2. 取出一个节点
            cur = q.get()
            for nei in cur.neighbors:
                if nei not in visited:
                    # 2.1 初始化一个新的克隆节点
                    visited[nei] = Node(nei.val, [])
                    q.put(nei)
                # 2.2 向克隆节点的邻居添加克隆邻居
                visited[cur].neighbors.append(visited[nei])
        return visited[node]
```


