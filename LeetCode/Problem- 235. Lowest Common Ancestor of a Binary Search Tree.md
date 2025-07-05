Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Tree]]

In this [[DSA]] problem, given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)

**Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
**Output:** 6
**Explanation:** The LCA of nodes 2 and 8 is 6.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)

**Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
**Output:** 2
**Explanation:** The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.

**Example 3:**

**Input:** root = [2,1], p = 2, q = 1
**Output:** 2

## Approach
When will a particular node be an ancestor of two different nodes?
When those 2 nodes will be located on either of it's sides, right?
And if those nodes are located on the same side? Well we check on that side with the same condition, and if the current node is equal to either of the nodes, it's the ancestor.
I figured this approach out after looking at all of the hints provided by NeetCode.

```
/**

* Definition for a binary tree node.

* struct TreeNode {

* int val;

* TreeNode *left;

* TreeNode *right;

* TreeNode(int x) : val(x), left(NULL), right(NULL) {}

* };

*/

  

class Solution {

public:

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {

if(root == p || root == q) return root;

if((p->val < root->val && q->val > root->val) || (p->val > root->val && q->val < root->val)) return root;

if(p->val < root->val) return lowestCommonAncestor(root->left, p, q);

return lowestCommonAncestor(root->right, p, q);

}

};
```
