Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[Stack]]

In this [[DSA]] question, design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

**Example 1:**

**Input**
`["MinStack","push","push","push","getMin","pop","top","getMin"]`
`[[],[-2],[0],[-3],[],[],[],[]]`

**Output**
`[null,null,null,null,-3,null,0,-2]`

**Explanation**
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2

## Approach 
I was able to come up with the solution to this on my own in 13 minutes. I'm getting better at finding solutions to problems I haven't solved by just taking some time to understand the problem properly and thinking of multiple approaches. I thought of maintaining just a single int variable, but then we were losing track of what the next smallest element in the stack will be upon popping the current smallest element.

The idea is to have 2 stacks. One will be the main stack. And the other will be the minStack. For every push operation, we will push to the main stack. But if the value of the element is less than or equal to the value of minStack.top(), we will push it to the minStack as well. Then, we can access the current minimum element in O(1) with minStack.top(). And while popping, if mainStack.top() == minStack.top() or minStack is empty, we pop from both the stacks. Else we just pop from the mainStack. This was a pretty nice problem to brainstorm.

### Code 
```
class MinStack {

public:

stack<int> main;

stack<int> min_stack;

  

MinStack() {}

void push(int val) {

main.push(val);

if(min_stack.empty() || val <= min_stack.top()) min_stack.push(val);

}

void pop() {

if(main.top() == min_stack.top()) min_stack.pop();

main.pop();

}

int top() {

return main.top();

}

int getMin() {

return min_stack.top();

}

};

  

/**

* Your MinStack object will be instantiated and called as such:

* MinStack* obj = new MinStack();

* obj->push(val);

* obj->pop();

* int param_3 = obj->top();

* int param_4 = obj->getMin();

*/
```
