# Evaluate Reverse Polish Notation

**Difficulty:** Medium

## Problem Statement

You are given an array of strings `tokens` that represents an arithmetic expression in Reverse Polish Notation.

Evaluate the expression. Return an integer that represents the value of the expression.

Note that:
- The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
- Each operand may be an integer or another expression.
- The division between two integers always truncates toward zero.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in Reverse Polish Notation.
- The answer and all the intermediate calculations can be represented in a 32-bit integer.

**Example 1:**
```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2:**
```
Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

**Example 3:**
```
Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
```

## Brief Explanation

Reverse Polish Notation uses postfix notation where operators follow operands. Use a stack to evaluate: push numbers, when encountering an operator, pop two operands, perform operation, push result.

## Approach 1: Stack (Optimal)

### Explanation
Use a stack to store operands. When encountering an operator, pop two operands, perform the operation, and push the result back.

### Pseudocode
```
stack = new Stack()
for each token in tokens:
    if token is operator:
        b = stack.pop()
        a = stack.pop()
        result = apply operator(a, b)
        stack.push(result)
    else:
        stack.push(parseInt(token))
return stack.pop()
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.Stack;

class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        
        for (String token : tokens) {
            if (token.equals("+") || token.equals("-") || 
                token.equals("*") || token.equals("/")) {
                int b = stack.pop();
                int a = stack.pop();
                int result = 0;
                switch (token) {
                    case "+":
                        result = a + b;
                        break;
                    case "-":
                        result = a - b;
                        break;
                    case "*":
                        result = a * b;
                        break;
                    case "/":
                        result = a / b;
                        break;
                }
                stack.push(result);
            } else {
                stack.push(Integer.parseInt(token));
            }
        }
        
        return stack.pop();
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <stack>
#include <vector>
#include <string>

class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> st;
        
        for (string token : tokens) {
            if (token == "+" || token == "-" || 
                token == "*" || token == "/") {
                int b = st.top();
                st.pop();
                int a = st.top();
                st.pop();
                int result = 0;
                if (token == "+") {
                    result = a + b;
                } else if (token == "-") {
                    result = a - b;
                } else if (token == "*") {
                    result = a * b;
                } else {
                    result = a / b;
                }
                st.push(result);
            } else {
                st.push(stoi(token));
            }
        }
        
        return st.top();
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Stack (Optimal) | O(n) | O(n) | Standard approach for RPN evaluation. Most efficient solution |

