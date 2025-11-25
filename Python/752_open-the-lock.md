# BFS Approach

- using BFS search all possible options until same state or deadends occurs
- Edge case -> the initial state itself can be in the deadends
- **Time Complexity** -> $O(10^4)$
  - Technically `O(1)`
- **Space Complexity** -> $O(10^4)$
  - Technically `O(1)`

```python
from typing import List
from collections import deque

class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        q = deque([("0000", 0)])
        deadends = set(deadends)
        seen = set()

        if "0000" in deadends:
            return -1

        while q:
            curr, turns = q.popleft()

            if curr == target:
                return turns

            for i in range(4):
                comb1 = curr[:i] + str((int(curr[i]) + 1) % 10) + curr[i+1:]
                if comb1 not in deadends and comb1 not in seen:
                    seen.add(comb1)
                    q.append((comb1, turns + 1))

                comb2 = curr[:i] + str((int(curr[i]) - 1) % 10) + curr[i+1:]
                if comb2 not in deadends and comb2 not in seen:
                    seen.add(comb2)
                    q.append((comb2, turns + 1))

        return -1

```
