# Greedy Approach

- all the complicated stuff in sorting
- Beats 62% in time and 17.2% in space
- **Time Complexity** -> $O(nlogn)$
- **Space Complexity** -> $O(n)$

```python
from typing import List

class Solution:
    def intersectionSizeTwo(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x: (x[1], -x[0]))
        res = []
        for i,j in intervals:
            if not res:
                res.append(j - 1)
                res.append(j)

            a, b = res[-1], res[-2]
            if a < i:
                res.append(j - 1)
                res.append(j)

            elif b < i:
                res.append(j)

        return len(res)

```

# Constant space approach

- just keeping track of the last 2 elements in the stack
- Beats 75% in time and 32.4% in space
- **Time Complexity** -> $O(nlogn)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def intersectionSizeTwo(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x: (x[1], -x[0]))
        res = 0
        a, b = -1, -1
        for i,j in intervals:
            if a < i:
                a, b = j, j-1
                res += 2

            elif b < i:
                a, b = j, a
                res += 1

        return res

```
