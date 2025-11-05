# Sum of top x most frequent elements of evry subset of size k

- the first part of this sum is easy as just use counter
- first hard after 6 months took around 1.2 hrs
- beats 100% in space but only 10% in time
- **Time Complexity** -> $O(n * k)$
- **Space Complexity** -> $O(n + k)$

```python

class Solution:
    def findXSum(self, nums: List[int], k: int, x: int) -> List[int]:
        freq = {}
        topx = SortedList(key = lambda x: (freq[x],x))
        rest = SortedList(key = lambda x: (freq[x],x))
        # list[0] -> lower priority
        # list[-1] -> higher priority
        topsum = [0]
        res = []

        def balance(x):
            # topx is not full
            while len(topx) < min(x, len(freq)) and rest:
                t = rest.pop()
                topx.add(t)
                topsum[0] += t * freq[t]

            # topx and rest same freq or topx have less freq
            if rest and ((freq[topx[0]] == freq[rest[-1]] and topx[0] < rest[-1]) or freq[topx[0]] < freq[rest[-1]]):
                r = topx.pop(0)
                t = rest.pop()
                topx.add(t)
                rest.add(r)
                topsum[0] += (t * freq[t]) - (r * freq[r])

        # first k
        for i in nums[:k]:
            if i in freq:
                if i in topx:
                    topx.remove(i)
                    topsum[0] -= i * freq[i]
                else:
                    rest.remove(i)

                freq[i] += 1
            else:
                freq[i] = 1

            rest.add(i)
            balance(x)
        res.append(topsum[0])

        # main window
        for i in range(k, len(nums)):
            # delete previous window element
            prev = nums[i-k]
            if freq[prev] == 1:
                if prev in topx:
                    topx.remove(prev)
                    topsum[0] -= prev * freq[prev]
                else:
                    rest.remove(prev)

                del freq[prev]

            else:
                if prev in topx:
                    topx.remove(prev)
                    topsum[0] -= prev * freq[prev]
                else:
                    rest.remove(prev)

                freq[prev] -= 1
                rest.add(prev)
                balance(x)

            # add new element
            curr = nums[i]
            if curr in freq:
                if curr in topx:
                    topx.remove(curr)
                    topsum[0] -= curr * freq[curr]
                else:
                    rest.remove(curr)

                freq[curr] += 1
            else:
                freq[curr] = 1

            rest.add(curr)
            balance(x)

            res.append(topsum[0])

        return res

```
