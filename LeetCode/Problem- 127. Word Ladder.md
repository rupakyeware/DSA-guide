Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Graph]], [[BFS]]

In this [[DSA]] problem, a **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _the **number of words**in the **shortest transformation sequence** from_ `beginWord` _to_ `endWord`_, or_ `0` _if no such sequence exists._

**Example 1:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
**Output:** 5
**Explanation:** One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.

**Example 2:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
**Output:** 0
**Explanation:** The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.

**Constraints:**

- `1 <= beginWord.length <= 10`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All the words in `wordList` are **unique**.

## Approach 
They key part of knowing how to solve this problem efficiently is being able to find a word's neighbours (words that differ from it by 1 character) efficiently. And for that, we'll use pattern matching. 

But to match the patterns, we first have to create the patterns. And that's what our adjacency list will store. The key will be the pattern and the value will be a vector of all the strings belonging to that pattern. 

We will find a word's patterns by replacing each of its characters with a * once and then pushing the word into that pattern's vector in the [[Unordered Map]].

Once the adjacency list is created, it's just a normal BFS now to find the shortest path to the endWord, if it exists.

### Code 
```cpp
class Solution {
public:
    unordered_map<string, vector<string>> adj;
    unordered_set<string> visited;
    
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        int m = beginWord.size();
        wordList.push_back(beginWord);
        
        for(string word: wordList) {
            for(int i = 0; i < m; i++) {
                string pattern = word.substr(0, i) + "*" + word.substr(i+1);
                adj[pattern].push_back(word);
            }    
        }

        queue<string> q;
        q.push(beginWord);
        int res = 0;
        
        while(!q.empty()) {
            int s = q.size();
            for(int i = 0; i < s; i++) {
                string curr = q.front();
                q.pop();

                if(curr == endWord) return res + 1;
                visited.insert(curr);

                for(int j = 0; j < m; j++) {
                    string pattern = curr.substr(0, j) + "*" + curr.substr(j+1);
                    for(string nb: adj[pattern]) {
                        if(visited.count(nb)) continue;
                        q.push(nb);
                    }
                }
            }
            res++;
        }

        return 0;
    }
};
```