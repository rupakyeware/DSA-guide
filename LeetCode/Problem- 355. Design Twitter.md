Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Unordered Map]], [[priority queue]], [[Stack]]

In this [[DSA]] problem, you have to design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the `10`most recent tweets in the user's news feed.

Implement the `Twitter` class:

- `Twitter()` Initializes your twitter object.
- `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
- `List<Integer> getNewsFeed(int userId)` Retrieves the `10` most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be **ordered from most recent to least recent**.
- `void follow(int followerId, int followeeId)` The user with ID `followerId` started following the user with ID `followeeId`.
- `void unfollow(int followerId, int followeeId)` The user with ID `followerId` started unfollowing the user with ID `followeeId`.

**Example 1:**

**Input**
```
["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
[[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]
```
**Output**
`[null, null, [5], null, null, [6, 5], null, [5]]`

**Explanation**
Twitter twitter = new Twitter();
twitter.postTweet(1, 5); // User 1 posts a new tweet (id = 5).
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> [5]. return [5]
twitter.follow(1, 2);    // User 1 follows user 2.
twitter.postTweet(2, 6); // User 2 posts a new tweet (id = 6).
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 2 tweet ids -> [6, 5]. Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.unfollow(1, 2);  // User 1 unfollows user 2.
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> [5], since user 1 is no longer following user 2.

**Constraints:**

- `1 <= userId, followerId, followeeId <= 500`
- `0 <= tweetId <= 104`
- All the tweets have **unique** IDs.
- At most `3 * 104` calls will be made to `postTweet`, `getNewsFeed`, `follow`, and `unfollow`.
- A user cannot follow himself.

## Approach-
This was a very fun problem to solve. I sort of approached it like a nosql database. For tracking which users a user is following, I made a hash map with the key as the userId and the value as a vector of ids of all the users they're following. 

For tracking the tweets, I made another hash map with the key as the user Id and the value as a vector of pairs where the pair will be of the timestamp and the tweetId.

The interesting part is fetching the tweets for getNewsFeed().
We want to fetch the 10 most recent feeds. So first, we make a vector of the users to be fetched. Then we iterate through that vector and add their tweets to a max heap.

Then we just have to pop the top 10 elements from the max heap into the posts vector.

### Code 
``` cpp
class Twitter {

public:

int time;

unordered_map<int, vector<pair<int, int>>> tweets;

unordered_map<int, vector<int>> following;

  

Twitter() {

time = 0;

}

void postTweet(int userId, int tweetId) {

tweets[userId].push_back({time, tweetId});

time++;

}

vector<int> getNewsFeed(int userId) {

vector<int> posts;

priority_queue<pair<int, int>> maxHeap;

vector<int> users_to_fetch = {userId}; // users whose posts we want to fetch

  

// add the users that the current user following to the list of users to be fetched

users_to_fetch.insert(users_to_fetch.end(), following[userId].begin(), following[userId].end());

  

// add 10 most recent tweets to a size limited min heap

for(int user: users_to_fetch) {

if(tweets[user].empty()) continue;

for(auto i: tweets[user]) {

maxHeap.push(i);

}

}

  

int count = 0;

// transfer tweets in minHeap to stack

while(!maxHeap.empty() && count < 10) {

auto tweet = maxHeap.top();

posts.push_back(tweet.second);

maxHeap.pop();

count++;

}

return posts;

}

void follow(int followerId, int followeeId) {

if(find(following[followerId].begin(), following[followerId].end(), followeeId) == following[followerId].end()) following[followerId].push_back(followeeId);

}

void unfollow(int followerId, int followeeId) {

if(find(following[followerId].begin(), following[followerId].end(), followeeId) == following[followerId].end()) return;

int idx_to_delete = find(following[followerId].begin(), following[followerId].end(), followeeId) - following[followerId].begin();

  

following[followerId].erase(following[followerId].begin() + idx_to_delete);

}

};

  

/**

* Your Twitter object will be instantiated and called as such:

* Twitter* obj = new Twitter();

* obj->postTweet(userId,tweetId);

* vector<int> param_2 = obj->getNewsFeed(userId);

* obj->follow(followerId,followeeId);

* obj->unfollow(followerId,followeeId);

*/
```