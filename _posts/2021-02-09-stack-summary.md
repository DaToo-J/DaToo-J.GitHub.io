---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 栈 总结
title: 栈总结
---


# 概要


1. 特点：Last In First Out，后入先出。（图来自 LeetCode 官方）
   ![queue]({{ "./assets/images/stack.gif" | absolute_url }}){:height="50%" width="50%"}

2. 操作：
   1. 入栈(push) ：也叫插入(insert)，新元素会被添加至栈顶；
   
   2. 出栈(pop) ：也叫删除(delete)，只能移除栈顶元素。

3. 应用：
   1. 「树的前、中、后序遍历」
   
   2.  「图的 DFS」，以上二者的特点：不达到最深的节点，就不回溯。DFS 因为要回溯，每次回溯的是最深的节点，也就是后进入的节点，这符合后入先出的特性。
   
   3. 递归：递归的调用栈，在某些时候，递归可以改写成栈的形式

4. DFS VS BFS

    |          | DFS                                      | BFS            |
    | -------- | ---------------------------------------- | -------------- |
    | 最常使用 | 栈                                       | 队列           |
    |          | 更早访问的结点可能不是更靠近根结点的结点 | 擅长找最短路径 |


# 模板

## 1. DFS 的递归模板

> 类似于回溯模板
> 
> 改写自 LeetCode 官方 Java 模板

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

## 2. DFS 的循环模板

> 将递归改写成「循环+栈」的形式
> 
> 改写自 LeetCode 官方 Java 模板

```python
def dfs(root, target):
    stack = [root]
    while stack:
        cur = stack.pop()
        if cur is target:
            return True

        for next in neighbors of cur:
            if next not in visited:
                stack.append(next)
                visited.add(next)
    return False
```

# 相关题目


   | No  | Problem          | Difficulty | Link                                                                    | Solution                                                              | Comment            |
   | --- | ---------------- | ---------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------- | ------------------ |
   | 94  | 二叉树的中序遍历 | 中等       | [题目](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/) | [题解]({% link _posts/2021-02-12-binary-tree-inorder-traversal.md %}) | 中序遍历的栈的实现 |
   | 200 | 岛屿数量         | 中等       | [题目](https://leetcode-cn.com/problems/number-of-islands/)             | [题解]({% link _posts/2021-02-11-numbers-of-islands.md %})            | DFS + 递归的解法   |
   | 200 | 岛屿数量         | 中等       | [题目](https://leetcode-cn.com/problems/number-of-islands/)             | [题解]({% link _posts/2021-02-11-num-of-islands.md %})                | DFS + 栈的解法     |
   | 739  | 每日温度 | 中等       | [题目](https://leetcode-cn.com/problems/daily-temperatures/) | [题解]({% link _posts/2021-02-12-daily-temperatures.md %}) | 栈的经典题目 |

