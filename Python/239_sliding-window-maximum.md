# Sliding window + heap + lazy deletion

- uses sliding window for lazy deletion and tracking window
- heap to effectively retrive max from a dynamic structure
- **Time Complexity** -> $O(n \ . \ logk)$
- **Space Complexity** -> $O(n)$

```python
from typing import List
import heapq
from collections import deque

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        res = []
        max_heap = []
        deleted = {}

        for i in range(k):
            heapq.heappush(max_heap, -nums[i])

        res.append(-max_heap[0])
        l = 0
        for r in range(k, len(nums)):
            heapq.heappush(max_heap, -nums[r])

            deleted[nums[l]] = deleted.get(nums[l], 0) + 1
            l += 1

            while deleted.get(-max_heap[0], 0):
                deleted[-max_heap[0]] -= 1
                heapq.heappop(max_heap)

            res.append(-max_heap[0])

        return res
```

# Sliding window + deque

- uses sliding window to track the window and deque to sort of do a monotonic stack to effective maintain max element
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(n)$

```python
from typing import List

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        queue = deque()
        res = []

        for i in range(k):
            while queue and queue[-1] < nums[i]:
                queue.pop()

            queue.append(nums[i])

        res.append(queue[0])
        for r in range(k, len(nums)):
            while queue and queue[-1] < nums[r]:
                queue.pop()

            queue.append(nums[r])
            if queue[0] == nums[r - k]:
                queue.popleft()

            res.append(queue[0])

        return res

```
