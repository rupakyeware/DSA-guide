Source: [[AlgoZenith]]
Difficulty: [[Hard]]
Topics: [[01BFS]], [[Graph]]

In this [[DSA]] problem, Monkey D. Luffy, on his journey to become the **King of Pirates**, must travel across the mysterious sea known as the **Grand Line**. The Grand Line is represented as an `N × M` grid. Each cell in the grid contains a **wind direction**, represented by a number from `1` to `4`:

### 🌬 Wind Directions:

- `1` → Wind flows **Right** (from `(i, j)` to `(i, j + 1)`)
    
- `2` → Wind flows **Left** (from `(i, j)` to `(i, j - 1)`)
    
- `3` → Wind flows **Down** (from `(i, j)` to `(i + 1, j)`)
    
- `4` → Wind flows **Up** (from `(i, j)` to `(i - 1, j)`)
    

Luffy starts at the **top-left corner** of the grid: `(0, 0)`  
He must reach the **bottom-right corner**: `(N - 1, M - 1)`

### 🚢 Sailing Rules:

- Luffy’s ship can **only move in the direction of the wind**.
    
- However, he can **change the wind direction in any cell** by paying a **cost of 1**.
    
- Each cell’s direction can be changed at most **once**.
    

### 🧠 Goal:

Find the **minimum total cost** required for Luffy to sail from `(0, 0)` to `(N - 1, M - 1)`.

---

## 📥 Input Format:

css

CopyEdit

`N M S[0][0] S[0][1] ... S[0][M-1] ... S[N-1][0] S[N-1][1] ... S[N-1][M-1]`

- `2 ≤ N, M ≤ 1000`
    
- `1 ≤ S[i][j] ≤ 4`
    

---

## 📤 Output Format:

Print the **minimum cost** to reach the bottom-right cell.

---

## 🧪 Sample Input 1:

CopyEdit

`4 4 1 1 1 1 2 2 2 2 1 1 1 1 2 2 2 2`

### ✅ Sample Output 1:

CopyEdit

`3`

---

## 🧪 Sample Input 2:

CopyEdit

`3 3 1 1 3 3 2 2 1 1 4`

### ✅ Sample Output 2:

CopyEdit

`0`

---

## 💡 Explanation (Sample 2):

There exists a path from `(0, 0)` to `(2, 2)` **without changing any wind directions**, so the cost is `0`.

## Approach 
This was a nice question to solve. The core approach was just a 01BFS but the important part was calculating the cost to travel to a particular neighbour. I had to really spend some time on that. All in all, it took me ~45 mins to come up with the solution and code it.

One neat trick I used is to arrange dx and dy such that the order matches with the value in grid representing the direction of the wind.

Meaning, if we are checking for the right neighbour (dx=0,dy=1), it will be in the 0th iteration of the for loop for finding coordinates of the neighbour. And while checking for the ith neighbour, if the value in the cell is i+1, then it means the cost to go to that neighbour is 0.

For eg. in the 0th iteration, where we are checking for the neighbour to the right, if the cost of current cell is 1, which means the wind is moving to the right, we can travel to the neighbour to the right for free! 

At first I wrote this using 4 if conditions, but then figured out a much more beautiful way to write it.

### Code 
``` cpp
// #include "/Users/rupakyeware/Desktop/DSA-Problems/stdc++.h"
#include<bits/stdc++.h>
 
#define int long long
#define endl '\n'
using namespace std;

typedef pair<int,int> state;

int n, m;
vector<vector<int>> g;
vector<vector<int>> dist;

vector<int> dx = {0, 0, 1, -1};
vector<int> dy = {1, -1, 0, 0};

vector<pair<state, int>> getNeighbours(state curr) {
    vector<pair<state,int>> neighbours;
    for(int i = 0; i < 4; i++) {
        int newRow = curr.first + dx[i]; // get dx of neighbour
        int newCol = curr.second + dy[i]; // get dy of neighbour

        if((newRow < 0 || newCol < 0) || (newRow >= n || newCol >= m)) continue; // if not valid neighbour

        if(i+1 == g[curr.first][curr.second]) neighbours.push_back({{newRow, newCol},0}); // if wind of current cell is going to neighbour, cost is 0
        else neighbours.push_back({{newRow, newCol},1}); // else cost is 1
    }

    return neighbours;
}

void bfs() {
    deque<state> dq;
    dq.push_back({0,0});
    dist[0][0] = 0;

    while(!dq.empty()) {
        state curr = dq.front();
        dq.pop_front();

        for(auto n: getNeighbours(curr)) {
            state next = n.first;
            int cost = n.second;

            if(dist[next.first][next.second] > dist[curr.first][curr.second] + cost) {
                dist[next.first][next.second] = dist[curr.first][curr.second] + cost;
                if(cost == 0) dq.push_front(next);
                else dq.push_back(next);
            }
        }
    }
}

void solve() {
    cin >> n >> m;
    g = vector<vector<int>> (n, vector<int> (m));
    dist = vector<vector<int>> (n, vector<int> (m, INT_MAX));

    for(int i = 0; i < n; i++) {
        for(int j = 0; j < m; j++) {
            cin >> g[i][j];
        }
    }

    bfs();

    cout << dist[n-1][m-1] << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);

    int _t = 1;
    // cin >> _t;

    while(_t--) solve();
    return 0;
}

```