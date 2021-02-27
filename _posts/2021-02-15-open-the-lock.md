---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"
comments: true

tags: BFS 队列 广度优先
title: 752 打开转盘锁
---

## 题目

> 题目链接：[752 打开转盘锁](https://leetcode-cn.com/problems/open-the-lock/)

你有一个带有四个圆形拨轮的转盘锁。每个拨轮都有10个数字： '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' 。每个拨轮可以自由旋转：例如把 '9' 变为  '0'，'0' 变为 '9' 。每次旋转都只能旋转一个拨轮的一位数字。

锁的初始数字为 '0000' ，一个代表四个拨轮的数字的字符串。

列表 deadends 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁定，无法再被旋转。

字符串 target 代表可以解锁的数字，你需要给出最小的旋转次数，如果无论如何不能解锁，返回 -1。

示例 1:

    输入：deadends = ["0201","0101","0102","1212","2002"], target = "0202"
    输出：6
    解释：
        可能的移动序列为 "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202"。
        注意 "0000" -> "0001" -> "0002" -> "0102" -> "0202" 这样的序列是不能解锁的，
        因为当拨动到 "0102" 时这个锁就会被锁定。

示例 2:

    输入: deadends = ["8888"], target = "0009"
    输出：1
    解释：
        把最后一位反向旋转一次即可 "0000" -> "0009"。

示例 3:

    输入: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
    输出：-1
    解释：
        无法旋转到目标数字且不被锁定。
示例 4:

    输入: deadends = ["0000"], target = "8888"
    输出：-1
    提示：
        死亡列表 deadends 的长度范围为 [1, 500]。
        目标数字 target 不会在 deadends 之中。
        每个 deadends 和 target 中的字符串的数字会在 10,000 个可能的情况 '0000' 到 '9999' 中产生。
 
## 思路 

1. 如果从 `"0000"` 转动一次，可以得到 `"1000", "9000", "0100", "0900", "0010", "0090", "0001", "0009"` 8 个新的密码。可看作是 BFS 每次搜索的空间。然后，再从这些密码继续进行 8 个方向的搜索，一定会遇到 `target`。

2. 需注意的是：

   1. 需要跳过 `deadends`

   2. 不能重复遍历已遍历过的密码，否则会陷入死循环

3. 参考：[网友题解](https://leetcode-cn.com/problems/open-the-lock/solution/wo-xie-liao-yi-tao-bfs-suan-fa-kuang-jia-jian-dao-/)
## 代码 

```python
class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        def addOne(s, idx):
            if s[idx] == "9":
                return s[:idx] + "0" + s[idx+1:]
            else:
                return s[:idx] + str(int(s[idx])+1) + s[idx+1:]
        def subOne(s, idx):
            if s[idx] == "0":
                return s[:idx] + "9" + s[idx+1:]
            else:
                return s[:idx] + str(int(s[idx])-1) + s[idx+1:]
                
        import queue
        q = queue.Queue()
        q.put("0000")
        
        # 防止重复遍历
        visited = {"0000"}

        # 记录转了几次
        level = 0

        while q.qsize() > 0:
            sz = q.qsize()
            # 整层整层地遍历
            for i in range(sz):
                cur = q.get()

                # 遇到 deadends 就跳过
                if cur in deadends: continue
                
                # 搜索到目标了
                if cur == target: return level
                
                # 可抵达状态：8 个方向
                for j in range(4):
                    up = addOne(cur, j)
                    # 只将没遍历过的密码添入队列
                    if up not in visited:
                        visited.add(up)
                        q.put(up)

                    down = subOne(cur, j)
                    # 只将没遍历过的密码添入队列
                    if down not in visited:
                        visited.add(down)
                        q.put(down)
            
            # 搜索层数 + 1
            level += 1

        return -1
```


