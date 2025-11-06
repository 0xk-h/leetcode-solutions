# DSU( Halfcompression ) + regions of min-heap from start

- slower since min-heap is maintaied at evry step which is unnecessary
- beats 36% in time and 90% in space
- **Time Complexity** -> $O(c logc + q)$
- **Space Complexity** -> $O(c)$

```python
import heapq
from typing import List

class Solution:
    def processQueries(self, c: int, connections: List[List[int]], queries: List[List[int]]) -> List[int]:
        par = [0] + [i for i in range(1, c+1)]
        status = [1] * (c+1)
        regions = {i: [i] for i in range(1, c+1)}
        res = []

        def find(n1):
            while par[n1] != n1:
                par[n1] = par[par[n1]]
                n1 = par[n1]

            return n1

        def union(n1, n2):
            p1 = find(n1)
            p2 = find(n2)
            if p1 != p2:
                if len(regions[p1]) > len(regions[p2]):
                    par[p2] = p1
                    for x in regions[p2]:
                        heapq.heappush(regions[p1], x)

                else:
                    par[p1] = p2
                    for x in regions[p1]:
                        heapq.heappush(regions[p2], x)

        for i,j in connections:
            union(i,j)

        for i,j in queries:
            if i == 2:
                status[j] = 0
            else:
                if status[j]:
                    res.append(j)
                else:
                    p = find(j)
                    while regions[p] and not status[regions[p][0]]:
                        heapq.heappop(regions[p])
                    if regions[p]:
                        res.append(regions[p][0])
                    else:
                        res.append(-1)

        return res

```

# DSU( Full pathcompression ) no heap

- Fastest but may have recursion overhead since its python
- no heap, dsu by rank then initialize regions(hashmap) as in other itself it is sorted do it in reverse so that pop is $O(1)$
- beats 94.36%% in time and 68.63% in space [2mb larger than previous one]
- **Time Complexity** -> $O(c + n + q)$
  - n -> len(connections)
  - q -> len(queries)
- **Space Complexity** -> $O(c)$

```python
from typing import List

class Solution:
    def processQueries(self, c: int, connections: List[List[int]], queries: List[List[int]]) -> List[int]:
        par = [i for i in range(c+1)]
        rank = [1] * (c+1)
        status = [1] * (c+1)
        res = []

        def find(n1):
            if par[n1] != n1:
                par[n1] = find(par[n1])
            return par[n1]

        def union(n1, n2):
            p1 = find(n1)
            p2 = find(n2)
            if p1 != p2:
                if rank[p1] > rank[p2]:
                    par[p2] = p1
                    rank[p1] += rank[p2]

                else:
                    par[p1] = p2
                    rank[p2] += rank[p1]

        for i,j in connections:
            union(i,j)

        for i in range(c+1):
            find(i)

        regions = {}
        i = len(par) -1
        for x in par[::-1]:
            if x in regions:
                regions[x].append(i)
            else:
                regions[x] = [i]
            i -= 1

        for i,j in queries:
            if i == 2:
                status[j] = 0
            else:
                if status[j]:
                    res.append(j)
                else:
                    p = find(j)
                    while regions[p] and not status[regions[p][-1]]:
                        regions[p].pop()
                    if regions[p]:
                        res.append(regions[p][-1])
                    else:
                        res.append(-1)


        return res

```
