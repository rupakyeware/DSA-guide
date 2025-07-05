Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Recursion]], [[Tree]], [[DFS]]

In this [[DSA]] problem, given a binary tree `root`, a node _X_ in the tree is named **good** if in the path from root to _X_ there are no nodes with a value _greater than_ X.

Return the number of **good** nodes in the binary tree.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/04/02/test_sample_1.png)**

**Input:** root = [3,1,4,3,null,1,5]
**Output:** 4
**Explanation:** Nodes in blue are **good**.
Root Node (3) is always a good node.
Node 4 -> (3,4) is the maximum value in the path starting from the root.
Node 5 -> (3,4,5) is the maximum value in the path
Node 3 -> (3,1,3) is the maximum value in the path.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/04/02/test_sample_2.png)**

**Input:** root = [3,3,null,4,2]
**Output:** 3
**Explanation:** Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.

**Example 3:**

**Input:** root = [1]
**Output:** 1
**Explanation:** Root is considered as **good**.

## Approach 
This was a bit of a tricky problem. I wasn't able to come up with the solution for this by myself. I learned something new though. I learned how we can pass parameters recursively in DFS because of this problem. 
The approach wasn't difficult to understand at all once I watched NeetCode's explanation.
We want to pass the maxValue we have encountered up till now to the child nodes.
If the current node has a value >= maxVal, it's a good node and it's value is 1.
We find the values of the left and right children this way and then return left + right + val of the current node.
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

int goodNodes(TreeNode* root) {

return DFS(root, root->val);

}

  

int DFS(TreeNode* root, int maxVal) {

if(!root) return 0;

int res = root->val >= maxVal ? 1 : 0;

maxVal = max(maxVal, root->val);

int count = DFS(root->left, maxVal) + DFS(root->right, maxVal);

return count + res;

}

};
```