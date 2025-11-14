# Brute force [ TLE ]

- just the regular using 3 for loops
- Got TLE
- **Time Complexity** -> $O(q \  . \ n^2)$
  - $O(q \  . \ n^2 \ + \ n^2)$
- **Space Complexity** -> $O(n^2)$

```python
from typing import List

class Solution:
    def rangeAddQueries(self, n: int, queries: List[List[int]]) -> List[List[int]]:
        mat = [[0]* n for _ in range(n)]
        for q in queries:
            for i in range(q[0], q[3] +1):
                for j in range(q[1], q[4] +1):
                    mat[i][j] += 1

        return mat

```

# Maintain diff arr only for rows

- barely passed but since changing every row during every query is very slow since 0 <= len(query) <= 10^5 whereas 0 <= n <= 500
- Beats 23.5% in time and 87% in space
- **Time Complexity** -> $O(q \  . \ n \ + \ n^2)$
- **Space Complexity** -> $O(n^2)$

```python
from typing import List

class Solution:
    def rangeAddQueries(self, n: int, queries: List[List[int]]) -> List[List[int]]:
        mat = [[0]* n for _ in range(n)]
        for r1, c1, r2, c2 in queries:
            for i in range(r1, r2 +1):
                mat[i][c1] += 1
                if c2 < n -1:
                    mat[i][c2 +1] -= 1

        for i in range(n):
            acc = 0
            for j in range(n):
                acc += mat[i][j]
                mat[i][j] = acc


        return mat

```

# Maintain diff for both row and column

- very similar to the previous one but just again creating an extra diff then from that recreating the diff mat obtained in the previous method
- Beats 95.77% in time and 87% in space
- **Time Complexity** -> $O(q \ + \ n^2)$
- **Space Complexity** -> $O(n^2)$

```python
from typing import List

class Solution:
    def rangeAddQueries(self, n: int, queries: List[List[int]]) -> List[List[int]]:
        mat = [[0]* n for _ in range(n)]
        for r1, c1, r2, c2 in queries:
            mat[r1][c1] += 1
            if c2 < n -1:
                mat[r1][c2 +1] -= 1

            if r2 < n -1:
                mat[r2 +1][c1] -= 1

            if c2 < n -1 and r2 < n -1:
                mat[r2 +1][c2 +1] += 1

        for j in range(n):
            acc = 0
            for i in range(n):
                acc += mat[i][j]
                mat[i][j] = acc

        for i in range(n):
            acc = 0
            for j in range(n):
                acc += mat[i][j]
                mat[i][j] = acc


        return mat

```
