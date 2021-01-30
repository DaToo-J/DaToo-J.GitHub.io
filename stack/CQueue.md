

## 思路

1. 思路一：两个辅助栈。一个记录 `push`  进去的元素；另一个只是在 `deleteHead`  时暂时保存 stack 的逆序来得到栈底元素，即队列头部元素。然后再将逆序的栈再逆序填回第一个栈。
2. 思路二：两个辅助栈。同样，一个栈 `self.stack`  记录 `push` 进去的元素；另一个栈 `self.helper`  在得到逆序元素之后，不再填回，等到该栈 `self.helper` 为空且需要`deleteHead` 时再将 `self.stack` 元素 `pop`  进来。
3. 思路一更符合直觉，思路二是优化，速度会快很多。


## 代码

```python
class CQueue:
    def __init__(self):
        self.stack = []
        self.helper = []

    def appendTail(self, value: int) -> None:
        self.stack.append(value)

    def deleteHead(self) -> int:
        if self.helper: return self.helper.pop()
        if not self.stack:return -1
        while self.stack:
            self.helper.append(self.stack.pop())
        return self.helper.pop(
```

