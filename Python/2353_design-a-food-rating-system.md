# Heap + reverse rating(decending order)

- Fastest of them all should have same time complexity as sortedList but way faster than it
- **Time Complexity** ->
- **Space Complexity** ->

```python
import heapq
from collections import defaultdict
from typing import List

class FoodRatings:

    def __init__(self, foods: List[str], cuisines: List[str], ratings: List[int]):
        self.rating = defaultdict(list)   # cuisine to highest rated
        self.food_to_cuisine = {}
        self.food_to_rating = {}

        for i in range(len(foods)):
            self.food_to_cuisine[foods[i]] = cuisines[i]
            heapq.heappush(self.rating[cuisines[i]], (-ratings[i], foods[i]))
            self.food_to_rating[foods[i]] = ratings[i]


    def changeRating(self, food: str, newRating: int) -> None:
        cuisine = self.food_to_cuisine[food]
        heapq.heappush(self.rating[cuisine], (-newRating, food))
        self.food_to_rating[food] = newRating


    def highestRated(self, cuisine: str) -> str:
        x = self.rating[cuisine][0]
        while self.food_to_rating[x[1]] != -x[0]:
            heapq.heappop(self.rating[cuisine])
            x = self.rating[cuisine][0]
        return self.rating[cuisine][0][1]

```

# SortedList + reverse rating(decending order)

- Still way slower than heap donoo why!
- **Time Complexity** ->
- **Space Complexity** ->

```python
from collections import defaultdict
from sortedcontainers import SortedList
from typing import List

class FoodRatings:

    def __init__(self, foods: List[str], cuisines: List[str], ratings: List[int]):
        self.rating = defaultdict(SortedList)   # cuisine to highest rated
        self.food_to_cuisine = {}
        self.food_to_rating = {}

        for i in range(len(foods)):
            self.food_to_cuisine[foods[i]] = cuisines[i]
            self.rating[cuisines[i]].add((-ratings[i], foods[i]))
            self.food_to_rating[foods[i]] = ratings[i]


    def changeRating(self, food: str, newRating: int) -> None:
        cuisine = self.food_to_cuisine[food]
        self.rating[cuisine].remove((-self.food_to_rating[food], food))
        self.rating[cuisine].add((-newRating, food))
        self.food_to_rating[food] = newRating

    def highestRated(self, cuisine: str) -> str:
        return self.rating[cuisine][0][1]

```

# SortedList + invert name (ascending order)

- slowest of them all the invert func slows it
- **Time Complexity** ->
- **Space Complexity** ->

```python
from collections import defaultdict
from sortedcontainers import SortedList
from typing import List


class FoodRatings:

    def __init__(self, foods: List[str], cuisines: List[str], ratings: List[int]):
        self.rating = defaultdict(SortedList)   # cuisine to highest rated
        self.food_to_cuisine = {}
        self.food_to_rating = {}

        for i in range(len(foods)):
            self.food_to_cuisine[foods[i]] = cuisines[i]
            self.rating[cuisines[i]].add((ratings[i], self.invert(foods[i])))
            self.food_to_rating[foods[i]] = ratings[i]


    def changeRating(self, food: str, newRating: int) -> None:
        inverted_name = self.invert(food)
        cuisine = self.food_to_cuisine[food]
        self.rating[cuisine].remove((self.food_to_rating[food], inverted_name))
        self.rating[cuisine].add((newRating, inverted_name))
        self.food_to_rating[food] = newRating

    def highestRated(self, cuisine: str) -> str:
        x = self.rating[cuisine][-1]
        return self.invert(x[1])



    def invert(self, food: str) -> str:
        # ord(a) = 97
        # ord(z) = 122
        # 97 + 122 = 219
        return "".join(chr(219 - ord(i)) for i in food)
    # output be like
    # a -> z
    # b -> y
    # z -> a

```
