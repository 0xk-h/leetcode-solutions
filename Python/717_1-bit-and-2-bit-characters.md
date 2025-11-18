# Approach

- question was very vague
- Beats 100% in time and 98.8% in space
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def isOneBitCharacter(self, bits: List[int]) -> bool:
        n = len(bits)
        i = 0

        while i < n -1:
            if bits[i] == 0:
                i += 1
            else:
                i += 2

        return i != n

```
