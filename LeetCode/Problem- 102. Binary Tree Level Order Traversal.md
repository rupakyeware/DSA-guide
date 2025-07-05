Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Tree]], [[Recursion]], [[BFS]]

In this [[DSA]] problem, given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** [3],[9,20],[15,7]

**Example 2:**

**Input:** root = [1]
**Output:** [1]

**Example 3:**

**Input:** root = []
**Output:** []

## Approach-
We basically just have to perform BFS and store the outputs in a 2D vector.
We initialize a queue with the head, if it exists. Else we return the empty vector.
Then we iterate until i < size of the queue at that instance.
We add the front element to the vector. Then we add the front element's children to the queue and pop the front element. We keep doing this until we reach an empty queue, which means we have traversed all the possible elements.

### Code 
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

vector<vector<int>> levelOrder(TreeNode* root) {

queue<TreeNode*> q;

vector<vector<int>> output;

if(!root) return output;

q.push(root);

  

while(!q.empty()) {

int size = q.size();

vector<int> currentLevel;

for(int i = 0; i < size; i++) {

TreeNode* node = q.front();

currentLevel.push_back(node->val);

if(node->left) q.push(node->left);

if(node->right) q.push(node->right);

q.pop();

}

output.push_back(currentLevel);

}

  

return output;

}

};
```
