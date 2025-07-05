Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[DFS]], [[Tree]]

In this [[DSA]] problem, given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

**Input:** root = [1,2,3,4,5]
**Output:** 3
**Explanation:** 3 is the length of the path [4,2,1,3] or [5,2,1,3].

**Example 2:**

**Input:** root = [1,2]
**Output:** 1

## Approach 
This problem is classified as [[Easy]] but it's closer to a [[Medium]]. 
The tricky part of this problem is figuring out that the value we are recursively returning and the final answer are 2 different things. 
What we are returning is the [[Problem- 104. Maximum Depth of Binary Tree |height]] of the tree. But what we have to calculate and output as the answer is the diameter.  So we will have two functions, a driver and the recursive function. 
The driver will call the recursive function. Inside the recursive function, we will calculate the max heights returned by the left and right children. 
The diameter can be calculated as `max_left + max_right`. We will maintain a global variable called max_diameter. If the newly calculated diameter value is > max_diameter, we will update.
Then we will return the height of the current node.
After all the recursive calls are done, the driver function will return max_diameter.

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

int maxDiameter = 0;

  

int DFS(TreeNode* root) {

if(!root) return 0;

int height_left = DFS(root->left);

int height_right = DFS(root->right);

maxDiameter = max(maxDiameter, height_left + height_right);

return max(height_left, height_right) + 1;

}

  

public:

int diameterOfBinaryTree(TreeNode* root) {

if(!root) return 0;

DFS(root);

return maxDiameter;

}

};
```
