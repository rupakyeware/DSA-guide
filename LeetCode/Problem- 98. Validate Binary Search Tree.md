Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[DFS]], [[Recursion]], [[Tree]]

In this [[DSA]] problem, given the `root` of a binary tree, _determine if it is a valid binary search tree (BST)_.

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

**Input:** root = [2,1,3]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

**Input:** root = [5,1,4,null,null,3,6]
**Output:** false
**Explanation:** The root node's value is 5 but its right child's value is 4.

## Approach
I was not able to figure out the solution to this by myself. Had to not only look at NeetCode's hints but also the video solution.

The approach is two perform a DFS while passing two values, a lower bound and an upper bound. 
While checking for the left child, the lower bound will be the same as the lower bound for the current node, but the upper bound will change to the value of the current node.
While checking for the right child, the upper bound will remain the same as the upper bound for the current node but the lower bound will change to the value of the current node.

Also because of the constraints of the value of the nodes, it's important to use long long everywhere. I used int in my first attempt and that resulted in a wrong answer.

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

bool isValidBST(TreeNode* root) {

if(!root) return true;

return DFS(root, LLONG_MIN, LLONG_MAX);

}

  

bool DFS(TreeNode* node, long long lo, long long hi) {

if(!node) return true;

if(node->val > lo && node->val < hi) {

return DFS(node->left, lo, node->val) && DFS(node->right, node->val, hi);

}

  

return false;

}

};
```