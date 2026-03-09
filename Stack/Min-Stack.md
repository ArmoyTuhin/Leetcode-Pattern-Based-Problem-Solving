# Min Stack

**Difficulty:** Medium

## Problem Statement

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

**Example 1:**
```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

## Brief Explanation

We need to track the minimum element in the stack. The challenge is maintaining the minimum when elements are popped.

## Approach 1: Two Stacks

### Explanation
Use two stacks: one for all elements and one to track minimums. When pushing, also push to min stack if it's smaller than or equal to current minimum.

### Pseudocode
```
class MinStack:
    stack = []
    minStack = []
    
    push(val):
        stack.push(val)
        if minStack is empty or val <= minStack.top():
            minStack.push(val)
    
    pop():
        if stack.top() == minStack.top():
            minStack.pop()
        stack.pop()
    
    top():
        return stack.top()
    
    getMin():
        return minStack.top()
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.Stack;

class MinStack {
    private Stack<Integer> stack;
    private Stack<Integer> minStack;
    
    public MinStack() {
        stack = new Stack<>();
        minStack = new Stack<>();
    }
    
    public void push(int val) {
        stack.push(val);
        if (minStack.isEmpty() || val <= minStack.peek()) {
            minStack.push(val);
        }
    }
    
    public void pop() {
        if (stack.peek().equals(minStack.peek())) {
            minStack.pop();
        }
        stack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <stack>

class MinStack {
private:
    stack<int> st;
    stack<int> minSt;
    
public:
    MinStack() {
    }
    
    void push(int val) {
        st.push(val);
        if (minSt.empty() || val <= minSt.top()) {
            minSt.push(val);
        }
    }
    
    void pop() {
        if (st.top() == minSt.top()) {
            minSt.pop();
        }
        st.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int getMin() {
        return minSt.top();
    }
};
```

</div>

**Time Complexity:** O(1) for all operations  
**Space Complexity:** O(n)

## Approach 2: Single Stack with Pairs (Optimal)

### Explanation
Store pairs of (value, minimum) in a single stack. Each element knows the minimum at the time it was pushed.

### Pseudocode
```
class MinStack:
    stack = []  // stores (value, minSoFar)
    
    push(val):
        if stack is empty:
            stack.push((val, val))
        else:
            minSoFar = min(val, stack.top().min)
            stack.push((val, minSoFar))
    
    pop():
        stack.pop()
    
    top():
        return stack.top().value
    
    getMin():
        return stack.top().min
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.Stack;

class MinStack {
    private Stack<int[]> stack;  // [value, minSoFar]
    
    public MinStack() {
        stack = new Stack<>();
    }
    
    public void push(int val) {
        if (stack.isEmpty()) {
            stack.push(new int[]{val, val});
        } else {
            int minSoFar = Math.min(val, stack.peek()[1]);
            stack.push(new int[]{val, minSoFar});
        }
    }
    
    public void pop() {
        stack.pop();
    }
    
    public int top() {
        return stack.peek()[0];
    }
    
    public int getMin() {
        return stack.peek()[1];
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <stack>
#include <vector>
#include <algorithm>

class MinStack {
private:
    stack<pair<int, int>> st;  // (value, minSoFar)
    
public:
    MinStack() {
    }
    
    void push(int val) {
        if (st.empty()) {
            st.push({val, val});
        } else {
            int minSoFar = min(val, st.top().second);
            st.push({val, minSoFar});
        }
    }
    
    void pop() {
        st.pop();
    }
    
    int top() {
        return st.top().first;
    }
    
    int getMin() {
        return st.top().second;
    }
};
```

</div>

**Time Complexity:** O(1) for all operations  
**Space Complexity:** O(n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Two Stacks | O(1) all ops | O(n) | Clear separation of concerns. Easy to understand |
| Single Stack with Pairs (Optimal) | O(1) all ops | O(n) | More space efficient. Each element stores its minimum. Both are optimal |

