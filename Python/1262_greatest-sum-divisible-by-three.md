# Approach

- 40000 is the maximum possible value and keep track of 2 sets of 2 lowest number one set is when divided by 3 gives 1 other 2
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def maxSumDivThree(self, nums: List[int]) -> int:
        res = 0

        r11 = 40000
        r12 = 40000
        r21 = 40000
        r22 = 40000

        for num in nums:
            if num % 3 == 1:
                if num < r11:
                    r11, r12 = num, r11
                elif num < r12:
                    r12 = num

            elif num % 3 == 2:
                if num < r21:
                    r21, r22 = num, r21
                elif num < r22:
                    r22 = num

            res += num

        if res % 3 == 0:
            return res

        elif res % 3 == 1:
            return res - min(r11, r21+r22)

        else:
            return res - min(r21, r11+r12)

```

# Custom Approach

- uses a custom sorting to always maintain the minimum 2 at front and tracking every element not divisible by 3 in pick
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(k)$
  - k is the no. of elements not divisible by 3

```python
from typing import List

class Solution:
    def maxSumDivThree(self, nums: List[int]) -> int:
        pick = []
        res = 0

        for i in nums:
            if i % 3 != 0:
                pick.append(i)
                if len(pick) >= 2:
                    if pick[0] >= i:
                        pick[0], pick[-1], pick[1] = pick[-1], pick[1], pick[0]
                    elif pick[1] > i:
                        pick[1], pick[-1] = pick[-1], pick[1]

            res += i

        need = res % 3

        if not need: return res
        if len(pick) < 2:
            return 0 if not pick or pick[0] % 3 != need else res - pick[0]

        x = pick[0] + pick[1]
        ans = float("inf")
        for i in pick:
            if i % 3 == need and i <= x:
                ans = min(ans, i)
        if ans != float("inf"):
            return res - ans

        return res - x if x % 3 == need else 0

```
