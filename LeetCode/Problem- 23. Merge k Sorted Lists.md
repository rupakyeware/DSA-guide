Source: [[Leetcode]]
Difficulty: [[Hard]]
Topics: [[Linked List]], [[priority queue]]

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

_Merge all the linked-lists into one sorted linked-list and return it._

**Example 1:**

**Input:** lists = `[[1,4,5],[1,3,4],[2,6]]`
**Output:** `[1,1,2,3,4,4,5,6]`
**Explanation:** The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6

**Example 2:**

**Input:** lists = []
**Output:** []

**Example 3:**

**Input:** lists = [[]]
**Output:** []

**Constraints:**

- `k == lists.length`
- `0 <= k <= 104`
- `0 <= lists[i].length <= 500`
- `-104 <= lists[i][j] <= 104`
- `lists[i]` is sorted in **ascending order**.
- The sum of `lists[i].length` will not exceed `104`.

## Approach 1- Inserting one by one after sorting
This is the approach I came up with which takes O(nlog(n)) time. We want to create a vector with all the values in the k linked lists. And then, we sort the entire list.

After that, we construct a new linked list from scratch with the elements from the vector one by one.

### Code 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    int binarySearch(vector<int> &ss, int valToFind) {
        int n = ss.size();
        int lo = 0, hi = n-1, ans = n;
        while(lo <= hi) {
            int mid = lo + ((hi-lo)/2);
            if(ss[mid] >= valToFind) {
                ans = mid;
                hi = mid-1;
            }
            else {
                lo = mid+1;
            }
        }

        return ans;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        int n = lists.size();
        if(n == 0) return nullptr;
        vector<int> vals;

        ListNode* curr;

        for(int i = 0; i < n; i++) {
            curr = lists[i];
            while(curr != nullptr) {
                // vals.insert(vals.begin() + binarySearch(vals, curr->val), curr->val);
                vals.push_back(curr->val);
                curr = curr->next;
            }
        }

        sort(vals.begin(), vals.end());
        
        ListNode* head = new ListNode();
        curr = head;

        for(int i = 0; i < vals.size(); i++) {
            ListNode *newNode = new ListNode(vals[i]);
            curr->next = newNode;
            curr = curr->next;
        }

        return head->next;
    }
};
```

## Approach 2- Using a [[priority queue]]
This approach takes just O(nlog(k)) and is very intuitive. 
Essentially, we will use a priority queue (min heap) and pop a ListNode. Since the node will be of the next minimum value, we will add it to our new Linked List and then if it has a next node, we will push it into the priority queue. This will go on as long as the priority queue isn't empty (which means all the nodes haven't been traversed yet).

### Code 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        auto cmp = [](ListNode* a, ListNode *b) { return a->val > b->val; };
        priority_queue<ListNode*, vector<ListNode*>, decltype(cmp)> pq;

        for(auto list: lists) {
            if(list != nullptr) pq.push(list);
        }

        ListNode *head = new ListNode();
        ListNode *curr = head;
        while(!pq.empty()) {
            ListNode *nextNode = pq.top();
            pq.pop();

            curr->next = nextNode;
            curr = curr->next;
            nextNode = nextNode->next;

            if(nextNode != nullptr) {
                pq.push(nextNode);
            }
        }

        return head->next;
    }
};
```