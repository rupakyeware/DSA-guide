Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Tree]], [[Recursion]], [[DFS]]

This [[DSA]] problem is built on top of [[Problem- 100. Same Tree]].

Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root`with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg)

**Input:** root = [3,4,5,1,2], subRoot = [4,1,2]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/28/subtree2-tree.jpg)

**Input:** root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
**Output:** false

## Approach
We need two functions in this problem. A driver and a helper. The driver will check if the current if subRoot is a subTree of the current tree. If not, does it exist in either the left or right child.
The way we check if they are subtrees is with the helper function which will check if the entire subtrees match of not.
The helper function is basically the solution we wrote in [[Problem- 100. Same Tree]].
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

bool isSubtree(TreeNode* root, TreeNode* subRoot) {

if(!subRoot) return true;

if(!root) return false;

  

if(isSameTree(root, subRoot)) return true;

return isSubtree(root->left, subRoot) || isSubtree(root->right, subRoot);

}

  

bool isSameTree(TreeNode* root, TreeNode* subRoot) {

if(!root && !subRoot) return true;

if(root && subRoot && root->val == subRoot->val) {

return isSameTree(root->left, subRoot->left) && isSameTree(root->right, subRoot->right);

}

  

return false;

}

};
```