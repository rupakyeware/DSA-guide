Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Stack]], [[String]]

In this [[DSA]] problem, you are given an _absolute_ path for a Unix-style file system, which always begins with a slash `'/'`. Your task is to transform this absolute path into its **simplified canonical path**.

The _rules_ of a Unix-style file system are as follows:

- A single period `'.'` represents the current directory.
- A double period `'..'` represents the previous/parent directory.
- Multiple consecutive slashes such as `'//'` and `'///'` are treated as a single slash `'/'`.
- Any sequence of periods that does **not match** the rules above should be treated as a **valid directory or** **file** **name**. For example, `'...'` and `'....'` are valid directory or file names.

The simplified canonical path should follow these _rules_:

- The path must start with a single slash `'/'`.
- Directories within the path must be separated by exactly one slash `'/'`.
- The path must not end with a slash `'/'`, unless it is the root directory.
- The path must not have any single or double periods (`'.'` and `'..'`) used to denote current or parent directories.

Return the **simplified canonical path**.

**Example 1:**

**Input:** path = "/home/"

**Output:** "/home"

**Explanation:**

The trailing slash should be removed.

**Example 2:**

**Input:** path = "/home//foo/"

**Output:** "/home/foo"

**Explanation:**

Multiple consecutive slashes are replaced by a single one.

**Example 3:**

**Input:** path = "/home/user/Documents/../Pictures"

**Output:** "/home/user/Pictures"

**Explanation:**

A double period `".."` refers to the directory up a level (the parent directory).

**Example 4:**

**Input:** path = "/../"

**Output:** "/"

**Explanation:**

Going one level up from the root directory is not possible.

**Example 5:**

**Input:** path = "/.../a/../b/c/../d/./"

**Output:** "/.../b/d"

**Explanation:**

`"..."` is a valid name for a directory in this problem.

**Constraints:**

- `1 <= path.length <= 3000`
- `path` consists of English letters, digits, period `'.'`, slash `'/'` or `'_'`.
- `path` is a valid absolute Unix path.

## Approach
I tried doing this with a pointer starting from the right and moving towards left but failed. The right intuition was to use a stack. But here, to make our job easier, we'll use a vector as a stack.

First we'll separate the string into pieces at the '/' using stringstream and getline. And then for every piece (word) of the string, we have to follow some conditional steps-
1. If word is empty (we had a // in string) - Just continue and ignore
2. If word == ".." and stack isn't empty (not at root dir right now) - Pop from stack 
3. Else if word != "." then it's a valid dir name - Push it to the stack 

Then we'll start building the result string on top of the root '/' dir and if the dir we're inserting isn't the first one, we'll push a / to the result before adding the dir itself.

## Code 
```cpp
class Solution {
public:
    string simplifyPath(string path) {
        vector<string> st;
        stringstream ss(path);
        string curr;
        while(getline(ss, curr, '/')) {
            if(curr.empty()) continue;
            if(curr == "..") {
                if(!st.empty()) st.pop_back();
            }
            else if(curr != ".") st.push_back(curr);
        }

        string ans = "/";
        for(int i = 0; i < st.size(); i++) {
            if(i) ans += "/";
            ans += st[i];
        }

        return ans;
    }
};
```