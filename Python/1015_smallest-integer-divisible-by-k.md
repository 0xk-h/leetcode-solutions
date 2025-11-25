# Standard Approach

- number with factors 2 and 5 is impossible
- iterate until theres a loop in the remainders or the remainder is 0
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(n)$

```python
from typing import List

class Solution:
    def smallestRepunitDivByK(self, k: int) -> int:
        if k % 2 == 0 or k % 5 == 0:
            return -1

        seen = {0}
        rem = 1 % k
        res = 1
        while rem not in seen:
            seen.add(rem)
            rem = ((rem * 10) + 1) % k
            res += 1

        return res if rem == 0 else -1

```

# Standard Approach

- there is always a loop in remainder before or equal to k
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def smallestRepunitDivByK(self, k: int) -> int:
        if k % 2 == 0 or k % 5 == 0:
            return -1

        rem = 0
        for i in range(k):
            rem = ((rem * 10) + 1) % k
            if rem == 0:
                return i + 1

        return -1

```
