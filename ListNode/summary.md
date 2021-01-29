1. 虚拟节点：
   1. 方案一：如果需要一个指针记录更新后的链表，但又不想使用 `head`  ，可以设置一个虚拟节点 `dummy`  ，虚拟节点的下一个节点指向最终的结果链表，此后遍历更新结果时使用 `.next` 来更新。
      1. `dummy` ：结果链表的虚拟节点，`dummy.next` 即为结果链表；
      2. `res` ：结果链表的指针；
      3. [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/) [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/) [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)
   2. 方案二：但有的时候不一定要给`dummy` 节点赋值，直接 `prev=None` ，通过遍历来赋值。
      1. [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

```python
# 方案一
dummy = ListNode(-1)
res = dummy         # 这样才可以被复制，指向同一个引用地址

while 某个条件:
    res.next = 更新结果链表的节点
    res = res.next
return dummy.next

# 方案二
prev, curr = None, head
while 某个条件：
    prev = XX
```

