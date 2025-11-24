# Bit Manupulation

- for `O(1)` space solution we make a two pass over the nums
- xor every number in nums and every possible number in nums
- so that every unique will be presented 2 times and cancel out since its xor and the repeated numbers presented 3 times
- find a bit where both numbers differ by taking `and( & )` on the xor and its 2's complement
- now again do the same thing but now sort based on the diff_bit
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def getSneakyNumbers(self, nums: List[int]) -> List[int]:
        n = len(nums)

        xor = 0
        for num in nums:
            xor ^= num

        for digit in range(n - 2):
            xor ^= digit

        # Rightmost different bit
        diff_bit = xor & -xor

        num1, num2 = 0, 0
        for num in nums:
            if num & diff_bit:
                num1 ^= num
            else:
                num2 ^= num

        for digit in range(n - 2):
            if digit & diff_bit:
                num1 ^= digit
            else:
                num2 ^= digit

        return [num1, num2]

```

# Standard Approach

- classic seen check using a hashset
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(n)$

```python
from typing import List

class Solution:
    def getSneakyNumbers(self, nums: List[int]) -> List[int]:
        seen = set()
        res = []

        for num in nums:
            if num in seen:
                res.append(num)

            seen.add(num)

        return res

```
