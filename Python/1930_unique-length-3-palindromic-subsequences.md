# Prefix suffix approach

- initialize left and right with values of leftmost and right most index of each element
- another loop to count the valid subsequence
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(n)$

```python
from typing import List

class Solution:
    def countPalindromicSubsequence(self, s: str) -> int:
        left = [len(s)] * 26
        right = [0] * 26
        res = 0

        for i, x in enumerate(s):
            ind = ord(x) - ord("a")

            left[ind] = min(left[ind], i)
            right[ind] = max(right[ind], i)

        for i in range(26):
            l, r = left[i], right[i]

            if r - l >= 2:

                seen = set()

                for j in range(l + 1, r):

                    if s[j] not in seen:
                        res += 1
                        seen.add(s[j])

        return res

```

# Prefix suffix + Bitmask

- Same but uses bitmasking to track same subsequence occurance
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def countPalindromicSubsequence(self, s: str) -> int:
        left = [len(s)] * 26
        right = [0] * 26
        res = 0

        for i, x in enumerate(s):
            ind = ord(x) - ord("a")

            left[ind] = min(left[ind], i)
            right[ind] = max(right[ind], i)

        for i in range(26):
            l, r = left[i], right[i]

            if r - l >= 2:
                seen = 0

                for j in range(l + 1, r):
                    ind = ord(s[j]) - ord("a")

                    if not (1 << ind) & seen:
                        res += 1
                        seen |= 1 << ind

        return res

```

# perfix suffix Approach

- Same but uses list to keep track since theres only 26 unique values possible
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$

```python
from typing import List

class Solution:
    def countPalindromicSubsequence(self, s: str) -> int:
        left = [len(s)] * 26
        right = [0] * 26
        res = 0

        for i, x in enumerate(s):
            ind = ord(x) - ord("a")

            left[ind] = min(left[ind], i)
            right[ind] = i

        for i in range(26):
            l, r = left[i], right[i]

            if r - l >= 2:
                seen = [0] * 26

                for j in range(l + 1, r):
                    ind = ord(s[j]) - ord("a")

                    if not seen[ind]:
                        res += 1
                        seen[ind] = 1

        return res

```

# Prefix, suffix

- Fastest among all since set conversions are built in C
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(1)$
  - since eventhough in worst case set(1: n) is possible that would be also at max 26 characters

```python
from typing import List

class Solution:
    def countPalindromicSubsequence(self, s: str) -> int:
        left = [len(s)] * 26
        right = [0] * 26
        res = 0

        for i, x in enumerate(s):
            ind = ord(x) - ord("a")

            left[ind] = min(left[ind], i)
            right[ind] = i

        for i in range(26):
            l, r = left[i], right[i]

            if r - l >= 2:
                res += len(set(s[l + 1: r]))

        return res

```
