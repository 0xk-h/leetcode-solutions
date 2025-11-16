# Using formula ((n \* n+1) / 2)

- fastest approach since only fewer ops done
- Beats 83% in time and 82.4% in space
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def numSub(self, s: str) -> int:
        MOD = 10**9 + 7
        res = 0
        cnt = 0
        for i in s:
            if i == "1":
                cnt += 1
            else:
                res += (cnt * (cnt + 1)) // 2
                cnt = 0

        if cnt:
            res += (cnt * (cnt + 1)) // 2

        return res % MOD

```

# Just addition

- same thing but implemented using only addition slower since more operations are done
- Beats 75% in time and 82.4% in space
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def numSub(self, s: str) -> int:
        MOD = 10**9 + 7
        res = 0
        cnt, acc = 0, 0
        for i in s:
            if i == "1":
                cnt += 1
                acc += cnt
            else:
                res += acc
                cnt = 0
                acc = 0

        if cnt:
            res += acc

        return res % MOD

```
