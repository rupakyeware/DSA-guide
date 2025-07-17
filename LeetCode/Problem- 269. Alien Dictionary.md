Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Graph]], [[DFS]], [[Topological Sort]]

In this [[DSA]] problem, there is a foreign language which uses the latin alphabet, but the order among letters is _not_ "a", "b", "c" ... "z" as in English.

You receive a list of _non-empty_ strings `words` from the dictionary, where the words are **sorted lexicographically** based on the rules of this new language. 

Derive the order of letters in this language. If the order is invalid, return an empty string. If there are multiple valid order of letters, return **any** of them.

A string `a` is lexicographically smaller than a string `b` if either of the following is true:

- The first letter where they differ is smaller in `a` than in `b`.
- There is no index `i` such that `a[i] != b[i]` _and_ `a.length < b.length`.

**Example 1:**

```java
Input: ["z","o"]

Output: "zo"
```

Explanation:  
From "z" and "o", we know 'z' < 'o', so return "zo".

**Example 2:**

```java
Input: ["hrn","hrf","er","enn","rfnn"]

Output: "hernf"
```

Explanation:

- from "hrn" and "hrf", we know 'n' < 'f'
- from "hrf" and "er", we know 'h' < 'e'
- from "er" and "enn", we know get 'r' < 'n'
- from "enn" and "rfnn" we know 'e'<'r'
- so one possibile solution is "hernf"

**Constraints:**

- The input `words` will contain characters only from lowercase `'a'` to `'z'`.
- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`


## Approach 
The key to this problem is forming the adjacency list and then using topological sort to traverse the nodes with the post order DFS.

To form the adjacency list, we will iterate on every consecutive pair of words and use two [[2 pointers]] to reach the first non-equal character among them. That will be the order. For eg. if the first pair of unequal characters among two words is n and a, then the relationship formed is n->a so to the list of n, we will push back a.

[[Topological Sort]] only works if the graph is a directed acyclic graph. So if there is a cycle, which we will detect using the visiting [[Unordered Set]], we will return "";
### Code 
``` cpp
class Solution {
public:
    unordered_map<char, vector<char>> adj;
    unordered_set<char> visited;
    unordered_set<char> visiting;
    string ans = "";

    bool postDFS(char node) {
        if(visiting.count(node)) return false;
        if(visited.count(node)) return true;
        visiting.insert(node);

        for(char nb: adj[node]) {
            if(!postDFS(nb)) return false;
        }

        visiting.erase(node);
        visited.insert(node);
        ans.push_back(node);

        return true;
    }

    string foreignDictionary(vector<string>& words) {
        string word1 = words[0];

        for(string &word: words) {
            for(char c: word) {
                adj[c];  // ensure all characters are in the graph
            }
        }

        for(int i = 1; i < words.size(); i++) {
            string word2 = words[i];
            int p1 = 0, p2 = 0;
            bool found = false;
            while(p1 < word1.size() && p2 < word2.size()) {
                if(word1[p1] != word2[p2]) {
                    found = true;
                    adj[word1[p1]].push_back(word2[p2]);
                    break;
                }
                p1++;
                p2++;
            }
            if(!found && p1 < word1.size()) return "";
            word1 = word2;
        }    

        for(auto &[src, _]: adj) {
            if(!visited.count(src)) {
                if(!postDFS(src)) return "";
            }
        }

        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```