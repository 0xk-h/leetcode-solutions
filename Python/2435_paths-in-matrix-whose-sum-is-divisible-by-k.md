# 3D DP

- for each transition store the count in the index corresponding to the remainder
- **Time Complexity** -> $O(n * m * k)$
- **Space Complexity** -> $O(n * m * k)$

```python
from typing import List

class Solution:
    def numberOfPaths(self, grid: List[List[int]], k: int) -> int:
        MOD = 10 ** 9 + 7
        n, m = len(grid), len(grid[0])
        res = [[[0 for _ in range(k)] for _ in range(m)] for _ in range(n)]
        res[0][0][grid[0][0] % k] = 1

        for i in range(1, n):
            for rem, val in enumerate(res[i - 1][0]):
                if val:
                    x = (rem + grid[i][0]) % k
                    res[i][0][x] = val

        for j in range(1, m):
            for rem, val in enumerate(res[0][j - 1]):
                if val:
                    x = (rem + grid[0][j]) % k
                    res[0][j][x] = val

        for i in range(1, n):
            for j in range(1, m):
                for rem, val in enumerate(res[i][j - 1]):
                    if val:
                        x = (rem + grid[i][j]) % k
                        res[i][j][x] += val

                for rem, val in enumerate(res[i - 1][j]):
                    if val:
                        x = (rem + grid[i][j]) % k
                        res[i][j][x] += val


        return res[-1][-1][0] % MOD

```
