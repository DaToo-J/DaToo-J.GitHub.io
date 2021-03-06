---
layout: single
classes: wide
toc: true
toc_label: "本文内容"
toc_icon: "list"
sidebar:
  nav: "main"

tags: 位运算 总结
title: 位运算总结
excerpt: 位运算的基本操作、相关题目的总结
---


# 一、常见操作

## 1. 异或 ⊕

### 1.1 计算

1. 表示：在数学中用 $⊕$ 表示，在代码中用 `^` 表示。

2. 计算：$a ⊕ b$ 是将 $a$ 和 $b$ 的二进制每一位进行运算（相同为 0，不同为 1）得出的数字。

### 1.2 性质


1. $a ⊕ a = 0$：任何数和其自身进行异或，等于 0

2. $a ⊕ 0 = a$：任何数和0进行异或，等于其本身

2. $a ⊕ b ⊕ c = a ⊕ c ⊕ b$：异或运算满足交换律

### 1.3 例子

> 实现两个数对调

1. 代码：
   ```python
   a = a ^ b
   b = a ^ b
   a = a ^ b
   ```

2. 推导：

   1. 第一行：等价于 $a = a ⊕ b$

   2. 第二行：

      1. 将上式带入等号右边 ⇒ $a ⊕ b = (a ⊕ b) ⊕ b$

      2. ∵ 异或运算满足交换律 ⇒  $a ⊕ b = a ⊕ (b ⊕ b)$

      3. ∵ 自身和自身异或为0 ⇒ $a ⊕ b = a ⊕ 0$

      4. ∵ 和0进行异或等于其本身⇒$a ⊕ b = a$

      5. ∴ 第二行的代码 $b = a ⊕ b = a$

   3. 第三行：

      1. 此时，$a = a ⊕ b, \ b = a$

      2. 带入等号右边 ⇒ $a ⊕ b = (a ⊕ b) ⊕ a$

      3. ∵ 交换律 ⇒ $a ⊕ b = (a ⊕ a) ⊕ b$

      4. ∵ 自身和自身异或为0 ⇒ $a ⊕ b = 0 ⊕ b$

      5. ∵和0进行异或等于其本身 ⇒$a ⊕ b = b$

      6. ∴ 第三行的代码 $a = a ⊕ b = b$

   4. 至此，实现两个数对调



# 二、相关题目

   | No           | Problem              | Difficulty | Link                                                        | Solution                                                  | Comment                                  |
   | ------------ | -------------------- | ---------- | ----------------------------------------------------------- | --------------------------------------------------------- | ---------------------------------------- |
   | 136          | 只出现一次的数字     | 简单       | [题目](https://leetcode-cn.com/problems/single-number/)     | [题解]({% link _posts/2021-03-15-single-number.md %})     | 「只出现一次的数字」系列之一             |
   | 137          | 只出现一次的数字 II  | 中等       | [题目](https://leetcode-cn.com/problems/single-number-ii/)  | [题解]({% link _posts/2021-03-16-single-number-ii.md %})  | 「只出现一次的数字」系列之二             |
   | 260          | 只出现一次的数字 III | 中等       | [题目](https://leetcode-cn.com/problems/single-number-iii/) | [题解]({% link _posts/2021-03-16-single-number-iii.md %}) | 「只出现一次的数字」系列之三             |
   | 338          | 比特位计数           | 中等       | [题目](https://leetcode-cn.com/problems/counting-bits/)     | [题解]({% link _posts/2021-03-03-counting-bits.md %})     | 找到规律并结合动态规划的思想即可解出题目 |
   | 面试题 08.04 | 幂集                 | 中等       | [题目](https://leetcode-cn.com/problems/power-set-lcci/)    | [题解]({% link _posts/2021-03-15-power-set-lcci.md %})    | 位运算和回溯算法都可以实现               |
   