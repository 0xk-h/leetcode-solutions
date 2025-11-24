# Bit Manupulation

- right shift and add the bit just as the explaination follows
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def prefixesDivBy5(self, nums: List[int]) -> List[bool]:
        res = []
        x = 0

        for bit in nums:
            x = (x << 1) | bit
            res.append(x % 5 == 0)

        return res

```

# Bit Manupulation revised

- Only store the remainder so that the number wont go too big
- Its faster since the only the remainders r used thus all calculation are with numbers in range(0..5)
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def prefixesDivBy5(self, nums: List[int]) -> List[bool]:
        res = []
        x = 0

        for bit in nums:
            x = ((x << 1) | bit) % 5
            res.append(x == 0)

        return res

```

# Standard Approach

- same comcept with no bit manupulation
- since `x << 1` is the same as `x * 2`
- and `x | bit` is the same as `x + bit`
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def prefixesDivBy5(self, nums: List[int]) -> List[bool]:
        res = []
        x = 0

        for bit in nums:
            x = ((x * 2) + bit) % 5
            res.append(x == 0)

        return res

```
