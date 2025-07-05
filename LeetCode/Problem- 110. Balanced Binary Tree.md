Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Tree]], [[Recursion]], [[DFS]]

In this [[DSA]] problem, given a binary tree, determine if it is **height-balanced**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

**Input:** root = [1,2,2,3,3,null,null,4,4]
**Output:** false

**Example 3:**

**Input:** root = []
**Output:** true

## Approach-
I approached this question similar to [[Problem- 543. Diameter of Binary Tree]]. I had to look at the hints to understand the exact approach though I had a basic idea of what the way to approach it was. 
We need one driver function which will return the final boolean, and one DFS function to calculate the heights recursively. 
I kept one global boolean variable initialized to true and at every step I calculated the height of the left and right children. If their difference was > 1, the boolean gets changed to false. We start by calling the recursive DFS from the driver function. And finally we return the boolean variable as that's our answer.

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

bool isValid = true;

  

int DFS(TreeNode* root) {

if(!root) return 0;

int height_left = DFS(root->left);

int height_right = DFS(root->right);

if(max(height_left, height_right) - min(height_left, height_right) > 1) isValid = false;

return max(height_left, height_right) + 1;

}

bool isBalanced(TreeNode* root) {

DFS(root);

return isValid;

}

};
```
