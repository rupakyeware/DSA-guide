Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[priority queue]]

In this [[DSA]] problem, you are given an array of integers `stones` where `stones[i]` is the weight of the `ith` stone.

We are playing a game with the stones. On each turn, we choose the **heaviest two stones** and smash them together. Suppose the heaviest two stones have weights `x` and `y` with `x <= y`. The result of this smash is:

- If `x == y`, both stones are destroyed, and
- If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`.

At the end of the game, there is **at most one** stone left.

Return _the weight of the last remaining stone_. If there are no stones left, return `0`.

**Example 1:**

**Input:** stones = [2,7,4,1,8,1]
**Output:** 1
**Explanation:** 
We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1] then,
we combine 2 and 4 to get 2 so the array converts to [2,1,1,1] then,
we combine 2 and 1 to get 1 so the array converts to [1,1,1] then,
we combine 1 and 1 to get 0 so the array converts to [1] then that's the value of the last stone.

**Example 2:**

**Input:** stones = [1]
**Output:** 1

**Constraints:**

- `1 <= stones.length <= 30`
- `1 <= stones[i] <= 1000`

## Approach 
We just have to maintain a min heap where first we would insert all the elements. Then while the size of the heap is greater than 1, we perform the operations given. If the size of the heap is less than 1 at the end, it means the last 2 stones popped were of the same size and ended up destroying each other, so we return 0. Else we return the top of the heap, which is also the only element in the heap.

### Code 
``` cpp
class Solution {

public:

priority_queue<int> pq;

  

int lastStoneWeight(vector<int>& stones) {

for(int s: stones) pq.push(s);

  

while(pq.size() > 1) {

int stone_1 = pq.top();

pq.pop();

int stone_2 = pq.top();

pq.pop();

  

if(stone_2 < stone_1) swap(stone_2, stone_1);

if(stone_1 == stone_2) continue;

else {

int new_stone = stone_2 - stone_1;

pq.push(new_stone);

}

  

}

  

if(pq.size() < 1) return 0;

return pq.top();

}

};
```

