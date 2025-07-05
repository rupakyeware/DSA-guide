Source: [[AlgoZenith]]
Difficulty: [[Medium]]
Topics: [[priority queue]], [[pair]], [[Array]]

In this [[DSA]] problem, Alice and Bob are playing a new game, which is very interesting and fun. The game is as follows:

1. The game starts with two n-sized integer arrays, A and B, and is played by two players, P1P1​ and P2P2​.
2. The players move in alternating turns, with P1P1​ always moving first. During each move, the current player must choose an integer, ii, such that 0≤i≤n−10≤i≤n−1.
    - If the current player is P1P1​, then P1P1​ receives Aipoints.
    - If the current player is P2P2​, then P2P2​ receives Bi​ points.
3. Each value of ii can be chosen only once. That is, if a value of ii is already chosen by some player, none of the players can re-use it. So, the game always ends after n moves.
4. The player with the maximum number of points wins.
5. The arrays AA and BB are accessible to both the players P1P1​ and P2P2​. So the players make an **optimal move** at every turn.

Given the values of n, A, and B, can you determine the outcome of the game? P1​ is Alice, and P2​ is Bob.  
Print **‘Alice’** if Alice will win, **'Bob'** if Bob will win, or **'Tie'** if they will tie. Assume both players always move _optimally_.

### Approach
I made the mistake of thinking that the most **optimal** approach for a player is to select the maximum value in their array. But that was a crucial mistake which I will not forget in future problems. 
Instead, the most optimal method is to maximize your **final** points. And the way to do that is by making sure you crush your opponents best options.
So each player will choose the option with max(A[i] + B[i]).

Then, the approach I used is to store these sum values in a heap of pairs along with the indexes of the respective sums. And then depending upon which player's turn it is, the player will select the top most element in the heap, add its points to themselves and then pop it.

I think we could reduce the execution time by calculating the sums, storing them in a vector and then sorting the vector. Although the time complexity remains the same, heaps would take more time as insertion takes log(n) in heap whereas it takes O(1) in vector. But then we do have to sort the vector which will take another nlog(n). This is why I think that if there is any advantage to this approach, it's extremely slight, but an advantage nonetheless.
### Code 
```
#include<bits/stdc++.h>

#define int long long

#define endl '\n'

using namespace std;

  

void solve() {

    int n; cin >> n;

    vector<int> vA(n), vB(n);

    priority_queue<pair<int, int>> best_element;

  

    for(int i = 0; i < n; i++) cin >> vA[i];

    for(int i = 0; i < n; i++) cin >> vB[i];

    for(int i = 0; i < n; i++) best_element.push({vA[i]+vB[i], i}); // Order best elements by A[i] + B[i] using heap

  

    int pointsA = 0, pointsB = 0;

  

    for(int i = 0; i < n; i++) {

        pair<int, int> best = best_element.top();

        if(i % 2 == 0) { // Add points to A

            pointsA += vA[best.second];

        }

        else { // Add points to B

            pointsB += vB[best.second];

        }

        best_element.pop();

    }

  

    if(pointsA > pointsB) cout << "Alice" << endl;

    else if(pointsB > pointsA) cout << "Bob" << endl;

    else cout << "Tie" << endl;

}

  

signed main() {

    ios_base::sync_with_stdio(false);

    cin.tie(NULL); cout.tie(NULL);

  

    int _t;

    cin >> _t;

  

    while(_t--) solve();

    return 0;

}
```