Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[Djikstra]], [[BFS]]

In this [[DSA]] problem, there is a **ball** in a maze with empty spaces and walls. The ball can go through empty spaces by rolling **up**, **down**, **left** or **right**, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's **start position**, the **destination** and the **maze**, find the shortest distance for the ball to stop at the destination. The distance is defined by the number of **empty spaces** traveled by the ball from the start position (excluded) to the destination (included). If the ball cannot stop at the destination, return -1.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.

**Example 1:**

**Input 1:** a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

**Input 2:** start coordinate (rowStart, colStart) = (0, 4)
**Input 3:** destination coordinate (rowDest, colDest) = (4, 4)

**Output:** 12

**Explanation:** One shortest way is : left -> down -> left -> down -> right -> down -> right.
             The total distance is 1 + 1 + 3 + 1 + 2 + 2 + 2 = 12.
![](https://assets.leetcode.com/uploads/2018/10/12/maze_1_example_1.png)

**Example 2:**

**Input 1:** a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

**Input 2:** start coordinate (rowStart, colStart) = (0, 4)
**Input 3:** destination coordinate (rowDest, colDest) = (3, 2)

**Output:** -1

**Explanation:** There is no way for the ball to stop at the destination.
![](https://assets.leetcode.com/uploads/2018/10/13/maze_1_example_2.png)

**Note:**

1. There is only one ball and one destination in the maze.
2. Both the ball and the destination exist on an empty space, and they will not be at the same position initially.
3. The given maze does not contain border (like the red rectangle in the example pictures), but you could assume the border of the maze are all walls.
4. The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.

## Approach
The approach and the intuition itself was pretty simple. Just the implementation took a while. We keep rolling in all directions from the current cell until we hit a wall, then we set the distance of the cell where we hit a wall to currDist + stepsTaken. I was trying to go cell by cell but chatgpt's approach of traversing until you hit a wall completely was much easier to implement.

### Code 
``` cpp
#include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
// #define int long long
#define endl '\n'
using namespace std;

int n, m;
vector<vector<int>> grid;
vector<vector<int>> dist;

typedef pair<int, int> state;

vector<int> dx = {-1, 0, 1, 0};
vector<int> dy = {0, 1, 0, -1};

bool isValid(state curr) {
    return ((curr.first >= 0 && curr.first < n) &&
            (curr.second >= 0 && curr.second < m) &&
            (grid[curr.first][curr.second] == 0));
}

void bfs(state start) {
    priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<tuple<int, int, int>>> pq;
    pq.push(make_tuple(0, start.first, start.second));
    dist[start.first][start.second] = 0;

    while (!pq.empty()) {
        tuple<int, int, int> top = pq.top();
        pq.pop();
        int cost = get<0>(top);
        int currX = get<1>(top);
        int currY = get<2>(top);

        for (int i = 0; i < 4; i++) {
            int newX = currX;
            int newY = currY;
            int steps = 0;
            while (isValid({newX + dx[i], newY + dy[i]})) {
                newX += dx[i];
                newY += dy[i];
                steps++;
            }

            if (cost + steps < dist[newX][newY]) {
                dist[newX][newY] = cost + steps;
                pq.push(make_tuple(dist[newX][newY], newX, newY));
            }
        }
    }
}

void solve() {
    cin >> n >> m;
    grid = vector<vector<int>>(n, vector<int>(m));
    dist = vector<vector<int>>(n, vector<int>(m, INT_MAX));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }

    state start;
    cin >> start.first >> start.second;
    state end;
    cin >> end.first >> end.second;

    bfs(start);

    cout << (dist[end.first][end.second] == INT_MAX ? -1 : dist[end.first][end.second]) << endl;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);

    int _t = 1;
    // cin >> _t;

    while (_t--) solve();
    return 0;
}
```
