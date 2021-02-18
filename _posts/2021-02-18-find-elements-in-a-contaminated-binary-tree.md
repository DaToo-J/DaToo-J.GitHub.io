---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 二叉树 哈希表
title: 1261 在受污染的二叉树中查找元素
---

## 题目

> 题目链接：1261 在受污染的二叉树中查找元素

给出一个满足下述规则的二叉树：

    root.val == 0
    如果 treeNode.val == x 且 treeNode.left != null，那么 treeNode.left.val == 2 * x + 1
    如果 treeNode.val == x 且 treeNode.right != null，那么 treeNode.right.val == 2 * x + 2
    现在这个二叉树受到「污染」，所有的 treeNode.val 都变成了 -1。

请你先还原二叉树，然后实现 FindElements 类：

    FindElements(TreeNode* root) 用受污染的二叉树初始化对象，你需要先把它还原。
    bool find(int target) 判断目标值 target 是否存在于还原后的二叉树中并返回结果。
 
示例 1：

    输入：
        ["FindElements","find","find"]
        [[[-1,null,-1]],[1],[2]]
    输出：
        [null,false,true]
    解释：
        FindElements findElements = new FindElements([-1,null,-1]); 
        findElements.find(1); // return False 
        findElements.find(2); // return True 


## 思路 

1. 还原二叉树：转化为二叉树的遍历，直接使用递归函数即可。

2. 查找元素：
   1. 先将还原后的二叉树的元素都 `+1`，然后将元素转化成二进制。
   2. 参考哈夫曼树的思想：从根节点到各元素经过的路径（向左为 `0`，向右为 `1`），拼成的一个二进制数就是该元素除了最高位的数，最高位为 `1`。
   3. 例子：节点 `11` 从根节点到该节点的路径：`左 -> 右 -> 右`，转成 `0 -> 1 -> 1`，加上最高位 `1` 得 `1011`，即为 `11` 的二进制

        ![hashmap]({{ "./assets/images/find-elements-in-a-contaminated-binary-tree.png" | absolute_url }})

3. 代码实现的步骤：
   1. `target`：`+1`
   2. 然后根据 `target` 除了最高位的剩余位的情况（`0` 或 `1`），向二叉树的左右子树移动。

## 代码 

### 二叉树的实现

```python
class FindElements:
    def __init__(self, root: TreeNode):
        self.root = self.rebuild(root, 0)

    def rebuild(self, root, val):
        if not root: return

        root.val = val
        if root.left:
            self.rebuild(root.left, 2 * val + 1)
        if root.right:
            self.rebuild(root.right, 2 * val + 2)
        return root

    def find(self, target: int) -> bool:
        if target < 0: return False

        target += 1
        bit = pow(2, len(bin(target))-4)
        node = self.root
        while bit > 0 and node != None:
            if target & bit == 0:
                node = node.left
            else:
                node = node.right
            bit >>= 1
        return node != None

```

### 哈希表的实现

```python
class FindElements:
    def __init__(self, root: TreeNode):
        def rebuild(root, val):
            if not root: return

            root.val = val
            self.val.add(val)
            if root.left:
                rebuild(root.left, 2 * val + 1)
            if root.right:
                rebuild(root.right, 2 * val + 2)
            return root
        
        self.val = set()
        self.root = rebuild(root, 0)

    def find(self, target: int) -> bool:
        return target in self.val
```        