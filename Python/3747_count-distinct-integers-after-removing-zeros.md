# Simple readable one but a little slow

- the no. of unique digits after remove zero in 1 digit is 9 and 2 digits is 81 (all are power of 9)
- Largest constraint ever seen 1 <= s <= 10^15
- 100% beats in time and space [Sorry, there are not enough accepted submissions to show data]
- **Time Complexity** -> O(1)
- **Space Complexity** -> O(1)

```python
from typing import List

class Solution:
    def countDistinct(self, s: int) -> int:
        s = str(s)
        n = len(s)
        res = 0

        # lesser digit than n
        pow_nine = 1
        for i in range(1, n):
            pow_nine *= 9
            res += pow_nine

        # same no. of digit as n
        for i in range(len(s)):
            if s[i] == "0":
                return res

            res += (int(s[i]) -1) * (9 ** (n - i -1))

        # n itself contains no zero
        return res + 1

```

# Using an arr instead of finding power of 9 everytime

- just initialize a arr of power of 9 where arr[i] = 9 \*\* i
- **Time Complexity** -> O(1)
- **Space Complexity** -> O(1)

```python
from typing import List

class Solution:
    def countDistinct(self, s: int) -> int:
        s = str(s)
        n = len(s)
        res = 0

        nine = [1] * (n +1)
        for i in range(1, n +1):
            nine[i] = nine[i -1] * 9

        # lesser digit than n
        for i in range(1, n):
            res += nine[i]

        # same no. of digit as n
        for i in range(len(s)):
            if s[i] == "0":
                return res

            res += (int(s[i]) -1) * nine[n - i -1]

        # n itself contains no zero
        return res + 1

```
