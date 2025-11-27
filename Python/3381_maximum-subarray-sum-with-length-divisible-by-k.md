# Prefix sum

- take prefix sum and group it with respect to its mod and minimize it as much as possible
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(k)$

```python
from typing import List

class Solution:
    def maxSubarraySum(self, nums: List[int], k: int) -> int:
        res = float("-inf")
        prefix = [float("inf")] * k
        prefix[0] = 0
        acc = 0

        for idx, num in enumerate(nums):
            acc += num
            mod = (idx + 1) % k
            res = max(res, acc - prefix[mod])
            prefix[mod] = min(prefix[mod], acc)

        return res

```
