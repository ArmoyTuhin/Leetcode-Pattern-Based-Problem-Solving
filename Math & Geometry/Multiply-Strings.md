# Multiply Strings

**Difficulty:** Medium

## Problem Statement

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

Note: You must not use any built-in BigInteger library or convert the inputs to integers directly.

**Example 1:**
```
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**
```
Input: num1 = "123", num2 = "456"
Output: "56088"
```

## Brief Explanation

We need to multiply two large numbers represented as strings without converting to integers. Use the standard multiplication algorithm we learn in school, digit by digit.

## Approach 1: Brute Force (Convert to Integer)

### Explanation
Convert strings to integers, multiply, convert back. This fails for very large numbers.

### Pseudocode
```
n1 = parseInt(num1)
n2 = parseInt(num2)
result = n1 * n2
return toString(result)
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
// Note: This approach fails for very large numbers
// Included for educational purposes only
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
// Note: This approach fails for very large numbers
// Included for educational purposes only
```

</div>

**Time Complexity:** O(1)  
**Space Complexity:** O(1)

## Approach 2: Standard Multiplication (Optimal)

### Explanation
Simulate the standard multiplication algorithm. For each digit in num2, multiply with each digit in num1, and add results at appropriate positions.

### Pseudocode
```
m = num1.length()
n = num2.length()
result = array of size m + n (all zeros)

for i = m-1 down to 0:
    for j = n-1 down to 0:
        mul = (num1[i] - '0') * (num2[j] - '0')
        p1 = i + j
        p2 = i + j + 1
        sum = mul + result[p2]
        result[p2] = sum % 10
        result[p1] += sum / 10

// Convert result array to string, removing leading zeros
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public String multiply(String num1, String num2) {
        int m = num1.length();
        int n = num2.length();
        int[] result = new int[m + n];
        
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
                int p1 = i + j;
                int p2 = i + j + 1;
                int sum = mul + result[p2];
                result[p2] = sum % 10;
                result[p1] += sum / 10;
            }
        }
        
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < result.length; i++) {
            if (!(sb.length() == 0 && result[i] == 0)) {
                sb.append(result[i]);
            }
        }
        
        return sb.length() == 0 ? "0" : sb.toString();
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <string>
#include <vector>

class Solution {
public:
    string multiply(string num1, string num2) {
        int m = num1.length();
        int n = num2.length();
        vector<int> result(m + n, 0);
        
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                int mul = (num1[i] - '0') * (num2[j] - '0');
                int p1 = i + j;
                int p2 = i + j + 1;
                int sum = mul + result[p2];
                result[p2] = sum % 10;
                result[p1] += sum / 10;
            }
        }
        
        string str;
        for (int i = 0; i < result.size(); i++) {
            if (!(str.empty() && result[i] == 0)) {
                str += to_string(result[i]);
            }
        }
        
        return str.empty() ? "0" : str;
    }
};
```

</div>

**Time Complexity:** O(m * n)  
**Space Complexity:** O(m + n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Convert to Integer | O(1) | O(1) | Not suitable for very large numbers (overflow) |
| Standard Multiplication (Optimal) | O(m * n) | O(m + n) | Most efficient. Handles large numbers correctly. Optimal solution |

