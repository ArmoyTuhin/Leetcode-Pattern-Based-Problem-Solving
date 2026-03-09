# Valid Parentheses

**Difficulty:** Easy

## Problem Statement

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**
```
Input: s = "()"
Output: true
```

**Example 2:**
```
Input: s = "()[]{}"
Output: true
```

**Example 3:**
```
Input: s = "(]"
Output: false
```

## Brief Explanation

We need to check if parentheses are properly matched and nested. Use a stack to track opening brackets and match them with closing brackets.

## Approach 1: Stack (Optimal)

### Explanation
Use a stack to store opening brackets. When encountering a closing bracket, check if it matches the top of the stack.

### Pseudocode
```
stack = new Stack()
for each char in s:
    if char is opening bracket:
        stack.push(char)
    else:
        if stack is empty:
            return false
        top = stack.pop()
        if top does not match char:
            return false
return stack is empty
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.Stack;

class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        
        for (char c : s.toCharArray()) {
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            } else {
                if (stack.isEmpty()) {
                    return false;
                }
                char top = stack.pop();
                if ((c == ')' && top != '(') ||
                    (c == '}' && top != '{') ||
                    (c == ']' && top != '[')) {
                    return false;
                }
            }
        }
        
        return stack.isEmpty();
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <stack>
#include <string>

class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        
        for (char c : s) {
            if (c == '(' || c == '{' || c == '[') {
                st.push(c);
            } else {
                if (st.empty()) {
                    return false;
                }
                char top = st.top();
                st.pop();
                if ((c == ')' && top != '(') ||
                    (c == '}' && top != '{') ||
                    (c == ']' && top != '[')) {
                    return false;
                }
            }
        }
        
        return st.empty();
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Stack (Optimal) | O(n) | O(n) | Standard approach for matching parentheses. Most efficient solution |

