# Pow(x, n)

**Difficulty:** Medium

## Problem Statement

Implement `pow(x, n)`, which calculates `x` raised to the power `n` (i.e., `xⁿ`).

**Example 1:**
```
Input: x = 2.00000, n = 10
Output: 1024.00000
```

**Example 2:**
```
Input: x = 2.10000, n = 3
Output: 9.26100
```

**Example 3:**
```
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2⁻² = 1/2² = 1/4 = 0.25
```

## Brief Explanation

We need to calculate x raised to power n efficiently. Use binary exponentiation (divide and conquer) to reduce time complexity from O(n) to O(log n).

## Approach 1: Brute Force

### Explanation
Multiply x by itself n times (or divide if n is negative).

### Pseudocode
```
if n < 0:
    x = 1 / x
    n = -n

result = 1
for i = 0 to n-1:
    result *= x
return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        
        double result = 1;
        for (long i = 0; i < N; i++) {
            result *= x;
        }
        return result;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        long long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        
        double result = 1;
        for (long long i = 0; i < N; i++) {
            result *= x;
        }
        return result;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

## Approach 2: Binary Exponentiation (Optimal)

### Explanation
Use divide and conquer. If n is even, xⁿ = (x²)^(n/2). If n is odd, xⁿ = x * x^(n-1).

### Pseudocode
```
if n < 0:
    x = 1 / x
    n = -n

function pow(x, n):
    if n == 0:
        return 1
    if n is even:
        return pow(x * x, n / 2)
    else:
        return x * pow(x * x, (n - 1) / 2)
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        return fastPow(x, N);
    }
    
    private double fastPow(double x, long n) {
        if (n == 0) {
            return 1.0;
        }
        double half = fastPow(x, n / 2);
        if (n % 2 == 0) {
            return half * half;
        } else {
            return half * half * x;
        }
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        long long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        return fastPow(x, N);
    }
    
private:
    double fastPow(double x, long long n) {
        if (n == 0) {
            return 1.0;
        }
        double half = fastPow(x, n / 2);
        if (n % 2 == 0) {
            return half * half;
        } else {
            return half * half * x;
        }
    }
};
```

</div>

**Time Complexity:** O(log n)  
**Space Complexity:** O(log n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n) | O(1) | Use only when n is very small |
| Binary Exponentiation (Optimal) | O(log n) | O(log n) | Most efficient. Reduces time complexity significantly. Optimal solution |

