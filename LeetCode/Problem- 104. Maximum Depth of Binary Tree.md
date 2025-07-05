Source: [[Leetcode]]
Difficulty: [[Easy]]
Topics: [[Recursion]], [[Tree]]

In this [[DSA]] problem, given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** 3

**Example 2:**

**Input:** root = [1,null,2]
**Output:** 2

## Approach
This was another very easy problem. At every node we traverse, we want to return the max of the depth of it's children + 1(counting itself). We traverse all the children recursively using [[DFS]].
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

int maxDepth(TreeNode* root) {

if(!root) return 0;

int depth_left = maxDepth(root->left);

int depth_right = maxDepth(root->right);

return 1 + max(depth_left, depth_right);

}

};
```