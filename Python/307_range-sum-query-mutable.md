# Fenwick Tree

- standard fenwick tree with O(n) build operation
- **Time Complexity:**
  - **init**() -> $O(n)$
  - update() -> $O(log n)$
  - sumRange() -> $O(log n)$
- **Space Complexity** -> $O(n)$

```python
from typing import List

class Fenwick():
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)

    def build(self, arr):
        for i in range(1, self.n + 1):
            self.tree[i] += arr[i - 1]
            par = i + (i & -i)
            if par < self.n + 1:
                self.tree[par] += self.tree[i]

    def update(self, i, x):
        while i < self.n + 1:
            self.tree[i] += x
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            res += self.tree[i]
            i -= i & -i

        return res

class NumArray:

    def __init__(self, nums: List[int]):
        self.nums = nums
        self.f = Fenwick(len(nums))
        self.f.build(nums)

    def update(self, index: int, val: int) -> None:
        self.f.update(index + 1, val - self.nums[index])
        self.nums[index] = val

    def sumRange(self, left: int, right: int) -> int:
        return self.f.query(right + 1) - self.f.query(left)


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(index,val)
# param_2 = obj.sumRange(left,right)

```
