Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Tree]], [[BFS]]

In this [[DSA]] problem, given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.

**Example 1:**

**Input:** root = [1,2,3,null,5,null,4]

**Output:** [1,3,4]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/11/24/tmpd5jn43fs-1.png)

**Example 2:**

**Input:** root = [1,2,3,4,null,null,null,5]

**Output:** [1,3,4,5]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/11/24/tmpkpe40xeh-1.png)

**Example 3:**

**Input:** root = [1,null,3]

**Output:** [1,3]

**Example 4:**

**Input:** root = []

**Output:** []

## Approach
This problem was very easy once I figured out that BFS was the way to go. We just have to print the last element at each level using BFS.
```
/**

* Definition for a binary tree node.

* struct TreeNode {

* int val;

* TreeNode *left;

* TreeNode *right;

* TreeNode() : val(0), left(nullptr), right(nullptr) {}

* TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}

* TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}

* };

*/

class Solution {

public:

vector<int> rightSideView(TreeNode* root) {

vector<int> output;

if(!root) return output;

  

queue<TreeNode*> q;

q.push(root);

while(!q.empty()) {

int size = q.size();

for(int i = 0; i < size; i++) {

TreeNode* node = q.front();

if(i == size - 1) {

output.push_back(node->val);

}

if(node->left) q.push(node->left);

if(node->right) q.push(node->right);

q.pop();

}

}

  

return output;

}

};
```
