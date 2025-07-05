Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Graph]], [[DFS]]

In this [[DSA]] problem 
### 📘 Description

Given a reference to a node in a **connected undirected graph**, return a **deep copy** (clone) of the graph.

Each node contains:

java

CopyEdit

`class Node {     public int val;     public List<Node> neighbors; }`

- You must return the copy of the given node as the reference to the **cloned graph**.
    
- The graph is represented using an **adjacency list** where:
    
    - Each element `adjList[i]` represents the neighbors of the node with `val = i + 1`.
        

---

### 📥 Input Format

- `adjList: List<List<Integer>>` — the adjacency list of the graph.
    
- The node with `val = 1` is always the starting point.
    
- Each node's value is the same as its 1-indexed position in the list.
    

---

### 📤 Output Format

- Return the cloned graph starting at the node with `val = 1`, in the same adjacency list format.
    

---

### ✅ Constraints

- `0 <= number of nodes <= 100`
    
- `1 <= Node.val <= 100`
    
- All `Node.val` values are unique.
    
- The graph is connected.
    
- No repeated edges or self-loops.
    

---

### 🔁 Examples

#### Example 1

lua

CopyEdit

`Input:  adjList = [[2,4],[1,3],[2,4],[1,3]] Output: [[2,4],[1,3],[2,4],[1,3]]`

**Explanation**:

- Node 1 connects to nodes 2 and 4.
    
- Node 2 connects to nodes 1 and 3.
    
- Node 3 connects to nodes 2 and 4.
    
- Node 4 connects to nodes 1 and 3.
    

#### Example 2

lua

CopyEdit

`Input:  adjList = [[]] Output: [[]]`

**Explanation**: One node with `val = 1` and no neighbors.

#### Example 3

makefile

CopyEdit

`Input:  adjList = [] Output: []`

**Explanation**: The graph is empty.

## Approach 
We approach this in a similar manner to cloning a tree. 
The difference is that in the tree we were first creating all the nodes and then attaching them to their child nodes.

Here we will recursively try to get the copy of the neighbors. If the copy has already been created, just return the address of the copied node. Else, create the copy along with its neighbors and return the address of the newly created copies node.

### Code 
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;
    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }
    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }
    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/

class Solution {
public:
    Node* cloneGraph(Node* node) {
        if(node == nullptr) return nullptr;

        unordered_map<Node*, Node*> copies;
        Node* copy = dfs(node, copies);
        return copy;
    }

    Node* dfs(Node* node, unordered_map<Node*, Node*> &copies) {
        if(copies.find(node) != copies.end()) return copies[node];

        Node* copy = new Node(node->val);
        copies[node] = copy;

        for(Node* nb: node->neighbors) {
            copy->neighbors.push_back(dfs(nb, copies));
        }

        return copy;
    }
};
```