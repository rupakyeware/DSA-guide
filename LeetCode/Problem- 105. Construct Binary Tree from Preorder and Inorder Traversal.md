Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Tree]], [[Recursion]]

In this [[DSA]] problem, given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
**Output:** [3,9,20,null,null,15,7]

**Example 2:**

**Input:** preorder = [-1], inorder = [-1]
**Output:** [-1]

**Constraints:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` and `inorder` consist of **unique** values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is **guaranteed** to be the preorder traversal of the tree.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.

## Approach 
The approach for this is pretty simple if you think about it.
What are the properties we know?
1. The first element of the preorder array will always be the root.
2. If the index of the first preorder element in the inorder array is mid, all elements to the left of mid will be to the left of root and all elements to the right of mid will be to the right of root.
These properties seem very obvious but the way we apply recursion on them isn't.
Essentially we want to make the first preorder element the root (or subroot), find the mid of the inorder list with it and then apply the same things all over again.

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

TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {

if(!preorder.size() || !inorder.size()) return nullptr;

TreeNode* root = new TreeNode(preorder[0]);

auto mid = find(inorder.begin(), inorder.end(), preorder[0]) - inorder.begin();

  

// make lhs and rhs vectors for inorder and preorder

vector<int> inorder_left(inorder.begin(), inorder.begin() + mid);

vector<int> preorder_left(preorder.begin() + 1, preorder.begin() + 1 + mid);

vector<int> inorder_right(inorder.begin() + mid + 1, inorder.end());

vector<int> preorder_right(preorder.begin() + mid + 1, preorder.end());

  

root->left = buildTree(preorder_left, inorder_left);

root->right = buildTree(preorder_right, inorder_right);

  

return root;

}

};
```
