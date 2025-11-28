# No Heap Approach

- instead of heap searches every followers including the user ten times to get the 10 most recent tweets
- **Time Complexity** -> $O(10 * f)$
- **Space Complexity** -> $O(n)$

```python
from typing import List

class Twitter:

    def __init__(self):
        self.follower = {}
        self.tweet = {}
        self.time = 0


    def postTweet(self, userId: int, tweetId: int) -> None:
        if userId in self.tweet:
            self.tweet[userId].append((self.time, tweetId))

        else:
            self.tweet[userId] = [(self.time, tweetId)]

        self.time += 1

    def getNewsFeed(self, userId: int) -> List[int]:
        res = []
        users = []

        if userId in self.tweet:
            users.append(userId)

        for user in self.follower.get(userId, []):
            if user in self.tweet:
                users.append(user)

        idx = [len(self.tweet[user]) - 1 for user in users]
        for _ in range(10):
            recent = (-1, -1)
            selected = -1
            for i in range(len(users)):
                index = idx[i]
                if index != -1 and self.tweet[users[i]][index][0] > recent[0]:
                    recent = self.tweet[users[i]][index]
                    selected = i

            if recent[0] == -1:
                return res

            res.append(recent[1])
            idx[selected] -= 1

        return res


    def follow(self, followerId: int, followeeId: int) -> None:
        if followerId in self.follower:
            self.follower[followerId].add(followeeId)

        else:
            self.follower[followerId] = {followeeId}


    def unfollow(self, followerId: int, followeeId: int) -> None:
        if followerId in self.follower:
            self.follower[followerId].remove(followeeId)

```

# Heap approach (unoptimized)

- takes every tweet from the followers and user. Heapify it and get the 10 most recent one
- **Time Complexity** -> $O(n)$
- **Space Complexity** -> $O(n)$

```python
from typing import List
import heapq

class Twitter:

    def __init__(self):
        self.tweet = {}
        self.follower = {}
        self.time = 0


    def postTweet(self, userId: int, tweetId: int) -> None:
        if userId in self.tweet:
            self.tweet[userId].append((self.time, tweetId))
        else:
            self.tweet[userId] = [(self.time, tweetId)]

        self.time -= 1


    def getNewsFeed(self, userId: int) -> List[int]:
        res = []
        heap = []

        if userId in self.follower:
            for user in self.follower[userId]:
                if user in self.tweet:
                    idx = len(self.tweet[user]) - 1
                    time, tweet = self.tweet[user][idx]
                    heap.append((time, tweet, user, idx))

        if userId in self.tweet:
            idx = len(self.tweet[userId]) - 1
            time, tweet = self.tweet[userId][idx]
            heap.append((time, tweet, userId, idx))

        heapq.heapify(heap)

        while heap and len(res) < 10:
            time, tweet, user, idx = heapq.heappop(heap)
            res.append(tweet)
            if idx > 0:
                time2, tweet2 = self.tweet[user][idx - 1]
                heapq.heappush(heap, (time2, tweet2, user, idx - 1))

        return res


    def follow(self, followerId: int, followeeId: int) -> None:
        if followerId in self.follower:
            self.follower[followerId].add(followeeId)
        else:
            self.follower[followerId] = {followeeId}


    def unfollow(self, followerId: int, followeeId: int) -> None:
        if followerId in self.follower and followeeId in self.follower[followerId]:
            self.follower[followerId].remove(followeeId)

```

# Heap Approach (Optimal)

- stores the most recent tweet from every follower in Heap and repeatedly pops the most recent one append to result and adds the 2nd recent from that user for atmost 10 times
- **Time Complexity** -> $O(f + 10 * logf)$
- **Space Complexity** -> $O(n)$

```python
from typing import List
import heapq

class Twitter:

    def __init__(self):
        self.follower = {}
        self.tweet = {}
        self.time = 0


    def postTweet(self, userId: int, tweetId: int) -> None:
        if userId in self.tweet:
            self.tweet[userId].append((self.time, tweetId))

        else:
            self.tweet[userId] = [(self.time, tweetId)]

        self.time -= 1

    def getNewsFeed(self, userId: int) -> List[int]:
        res = []
        feed = []

        feed.extend(self.tweet.get(userId, []))
        for user in self.follower.get(userId, []):
            feed.extend(self.tweet.get(user, []))

        heapq.heapify(feed)
        for _ in range(min(len(feed), 10)):
            _, tweet = heapq.heappop(feed)
            res.append(tweet)

        return res


    def follow(self, followerId: int, followeeId: int) -> None:
        if followerId in self.follower:
            self.follower[followerId].add(followeeId)

        else:
            self.follower[followerId] = {followeeId}


    def unfollow(self, followerId: int, followeeId: int) -> None:
        if followerId in self.follower:
            self.follower[followerId].remove(followeeId)

```
