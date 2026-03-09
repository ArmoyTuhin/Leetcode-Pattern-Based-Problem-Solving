# Happy Number

**Difficulty:** Easy

## Problem Statement

Write an algorithm to determine if a number `n` is happy.

A happy number is a number defined by the following process:
- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
- Those numbers for which this process ends in 1 are happy.

Return `true` if `n` is a happy number, and `false` if not.

**Example 1:**
```
Input: n = 19
Output: true
Explanation:
1² + 9² = 82
8² + 2² = 68
6² + 8² = 100
1² + 0² + 0² = 1
```

**Example 2:**
```
Input: n = 2
Output: false
```

## Brief Explanation

We need to check if repeatedly summing squares of digits eventually reaches 1. Use a set to detect cycles, or use Floyd's cycle detection algorithm.

## Approach 1: Hash Set

### Explanation
Use a set to track visited numbers. If we encounter a number we've seen before (cycle), return false. If we reach 1, return true.

### Pseudocode
```
seen = new HashSet()
current = n

while current != 1:
    if current in seen:
        return false
    seen.add(current)
    current = sumOfSquares(current)
return true
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public boolean isHappy(int n) {
        Set<Integer> seen = new HashSet<>();
        
        while (n != 1) {
            if (seen.contains(n)) {
                return false;
            }
            seen.add(n);
            n = sumOfSquares(n);
        }
        
        return true;
    }
    
    private int sumOfSquares(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <unordered_set>

class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> seen;
        
        while (n != 1) {
            if (seen.find(n) != seen.end()) {
                return false;
            }
            seen.insert(n);
            n = sumOfSquares(n);
        }
        
        return true;
    }
    
private:
    int sumOfSquares(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }
};
```

</div>

**Time Complexity:** O(log n)  
**Space Complexity:** O(log n)

## Approach 2: Floyd's Cycle Detection (Optimal)

### Explanation
Use two pointers (slow and fast) to detect cycles without using extra space. If fast pointer reaches 1, it's happy. If slow and fast meet, there's a cycle.

### Pseudocode
```
slow = n
fast = sumOfSquares(n)

while fast != 1 and slow != fast:
    slow = sumOfSquares(slow)
    fast = sumOfSquares(sumOfSquares(fast))

return fast == 1
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = sumOfSquares(n);
        
        while (fast != 1 && slow != fast) {
            slow = sumOfSquares(slow);
            fast = sumOfSquares(sumOfSquares(fast));
        }
        
        return fast == 1;
    }
    
    private int sumOfSquares(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
class Solution {
public:
    bool isHappy(int n) {
        int slow = n;
        int fast = sumOfSquares(n);
        
        while (fast != 1 && slow != fast) {
            slow = sumOfSquares(slow);
            fast = sumOfSquares(sumOfSquares(fast));
        }
        
        return fast == 1;
    }
    
private:
    int sumOfSquares(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }
};
```

</div>

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Hash Set | O(log n) | O(log n) | Simple to understand. Easy to implement |
| Floyd's Cycle Detection (Optimal) | O(log n) | O(1) | Most efficient. No extra space needed. Optimal solution |

