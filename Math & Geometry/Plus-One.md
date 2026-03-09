# Plus One

**Difficulty:** Easy

## Problem Statement

You are given a large integer represented as an integer array `digits`, where each `digits[i]` is the `i-th` digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading zeros.

Increment the large integer by one and return the resulting array of digits.

**Example 1:**
```
Input: digits = [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
Incrementing by one gives 123 + 1 = 124.
Thus, the result should be [1,2,4].
```

**Example 2:**
```
Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].
```

## Brief Explanation

We need to add 1 to a number represented as an array of digits. Handle carry propagation from right to left, and handle the case where all digits are 9.

## Approach 1: Convert to Number (Not Suitable for Large Numbers)

### Explanation
Convert array to number, add 1, convert back. This fails for very large numbers that exceed integer limits.

### Pseudocode
```
num = 0
for each digit in digits:
    num = num * 10 + digit
num++
convert num back to array
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

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

## Approach 2: Direct Manipulation (Optimal)

### Explanation
Start from the rightmost digit. Add 1 and propagate carry. If all digits are 9, create a new array with leading 1.

### Pseudocode
```
n = digits.length
for i = n-1 down to 0:
    if digits[i] < 9:
        digits[i]++
        return digits
    digits[i] = 0

// All digits were 9
result = new array of size n+1
result[0] = 1
return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int[] plusOne(int[] digits) {
        int n = digits.length;
        
        for (int i = n - 1; i >= 0; i--) {
            if (digits[i] < 9) {
                digits[i]++;
                return digits;
            }
            digits[i] = 0;
        }
        
        // All digits were 9
        int[] result = new int[n + 1];
        result[0] = 1;
        return result;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>

class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int n = digits.size();
        
        for (int i = n - 1; i >= 0; i--) {
            if (digits[i] < 9) {
                digits[i]++;
                return digits;
            }
            digits[i] = 0;
        }
        
        // All digits were 9
        vector<int> result(n + 1, 0);
        result[0] = 1;
        return result;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(1) excluding output for new array case

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Convert to Number | O(n) | O(1) | Not suitable for large numbers (overflow) |
| Direct Manipulation (Optimal) | O(n) | O(1) | Most efficient. Handles large numbers correctly. Optimal solution |

