Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Tree]], [[Recursion]], [[Array]]

In this [[DSA]] problem, given the `root` of a binary search tree, and an integer `k`, return _the_ `kth` _smallest value (**1-indexed**) of all the values of the nodes in the tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

**Input:** root = [3,1,4,null,2], k = 1
**Output:** 1

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

**Input:** root = [5,3,6,2,4,null,null,1], k = 3
**Output:** 3

I was not able to come up with the approach for this myself.
I looked at the hints to come up with the first approach.
## Approach 1- Append values to a vector and then return k-1th element
I learned how to integrate a vector into a recursion based tree problem because of this question.
We basically append all the elements in a tree to a vector in in-order DFS traversal.
And then from the driver function, we return the k-1th element in the vector as the vector will already have sorted elements because of our in order dfs method of traversal.
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

vector<int> v;

int kthSmallest(TreeNode* root, int k) {

DFS(root);

return v[k-1];

}

  

void DFS(TreeNode* node) {

if(node->left) DFS(node->left);

v.push_back(node->val);

if(node->right) DFS(node->right);

}

};
```

## Approach 2- Using stack and iterative DFS
I had to really focus and dry run the solution code to understand this approach. But it helped me a realize a major case I was missing which is- if a node has a right child, we might be able to find some values if the right child has left children.

In this approach, we perform iterative DFS on a stack. After we finish adding the left children, we will pop an element, set it as ans and then decrement k. If k = 0 we return ans. Else we **move  curr to the right child**
If curr is not null, we add the left children again until curr is null.
But if it's null from the beginning, which means there wasn't a right child and the next element in the stack is the smallest one, we pop from the stack, update ans and decrement k. Then we repeat this over and over until k == 0, which means we go the right ans.

This approach is so much better! However, it's also a little complex so it will need some practise to get used to. My vector approach can work as a back up.

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

int kthSmallest(TreeNode* root, int k) {

TreeNode* curr = root;

stack<TreeNode*> st;

while(!st.empty() || curr != nullptr) {

while(curr != nullptr) {

st.push(curr);

curr = curr->left;

}

curr = st.top();

st.pop();

k--;

if(k == 0) break;

curr = curr->right;

}

return curr->val;

}

  

};
```