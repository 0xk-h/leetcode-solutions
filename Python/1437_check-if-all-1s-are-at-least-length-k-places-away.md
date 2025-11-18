# Approach

- Beats 100% in time and 66.7% in space
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def kLengthApart(self, nums: List[int], k: int) -> bool:
        cnt = 0

        for i in nums:
            if i:
                if cnt > 0:
                    return False
                else:
                    cnt = k
            else:
                cnt -= 1

        return True

```
