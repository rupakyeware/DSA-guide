Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Tree]], [[DFS]], [[Recursion]]

In this [[DSA]] problem, given the `root` of a binary tree, invert the tree, and return _its root_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

**Input:** root = [4,2,7,1,3,6,9]
**Output:** [4,7,2,9,6,3,1]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

**Input:** root = [2,1,3]
**Output:** [2,3,1]

**Example 3:**

**Input:** root = []
**Output:** []

## Approach- [[Recursion | Recursively]] call each node and swap it's children
This is a very easy problem. We just have to visit each node, swap it's children and then visit the children using [[DFS]] by recursively calling the function with the children. We keep doing this until all the children have been traversed.
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

TreeNode* invertTree(TreeNode* root) {

if(!root) return nullptr;

swap(root->left, root->right);

invertTree(root->left);

invertTree(root->right);

return root;

}

};
```