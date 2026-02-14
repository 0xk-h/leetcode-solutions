# Fenwick tree Approach

- give rank based on sorted order [duplicates have same rank]
- store those numbers at the tree as rank as thier index
- **Time Complexity** -> $O(n logn)$
- **Space Complexity** -> $O(n)$

```python
from typing import List

class Fenwick():
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (self.n + 1)

    def update(self, i, val):
        while i <= self.n:
            self.tree[i] += val
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            res += self.tree[i]
            i -= i & -i

        return res

class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        n = len(nums)
        f = Fenwick(n)
        res = [0] * n
        rank = {}

        for val in sorted(set(nums)):
            rank[val] = len(rank) + 1

        for i in range(n - 1, -1, -1):
            x = rank[nums[i]]
            res[i] = f.query(x - 1)
            f.update(x, 1)

        return res

```
