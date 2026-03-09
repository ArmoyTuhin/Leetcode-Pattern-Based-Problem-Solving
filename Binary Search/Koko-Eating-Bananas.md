# Koko Eating Bananas

**Difficulty:** Medium

## Problem Statement

Koko loves to eat bananas. There are `n` piles of bananas, the `i-th` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko wants to finish eating all the bananas before the guards come back.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

**Example 1:**
```
Input: piles = [3,6,7,11], h = 8
Output: 4
```

**Example 2:**
```
Input: piles = [30,11,23,4,20], h = 5
Output: 30
```

**Example 3:**
```
Input: piles = [30,11,23,4,20], h = 6
Output: 23
```

## Brief Explanation

We need to find the minimum eating speed k such that Koko can finish all bananas in h hours. Use binary search on the possible speed values.

## Approach 1: Linear Search

### Explanation
Try each possible speed from 1 to max(piles) and check if it's valid.

### Pseudocode
```
maxPile = max(piles)
for speed = 1 to maxPile:
    if canFinish(piles, speed, h):
        return speed
return maxPile
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int maxPile = 0;
        for (int pile : piles) {
            maxPile = Math.max(maxPile, pile);
        }
        
        for (int speed = 1; speed <= maxPile; speed++) {
            if (canFinish(piles, speed, h)) {
                return speed;
            }
        }
        return maxPile;
    }
    
    private boolean canFinish(int[] piles, int speed, int h) {
        int hours = 0;
        for (int pile : piles) {
            hours += (pile + speed - 1) / speed; // Ceiling division
        }
        return hours <= h;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int maxPile = *max_element(piles.begin(), piles.end());
        
        for (int speed = 1; speed <= maxPile; speed++) {
            if (canFinish(piles, speed, h)) {
                return speed;
            }
        }
        return maxPile;
    }
    
private:
    bool canFinish(vector<int>& piles, int speed, int h) {
        int hours = 0;
        for (int pile : piles) {
            hours += (pile + speed - 1) / speed; // Ceiling division
        }
        return hours <= h;
    }
};
```

</div>

**Time Complexity:** O(max(piles) * n)  
**Space Complexity:** O(1)

## Approach 2: Binary Search (Optimal)

### Explanation
Use binary search on the possible speed range [1, max(piles)]. For each speed, check if it's valid. If valid, try smaller speeds; otherwise, try larger speeds.

### Pseudocode
```
left = 1
right = max(piles)
result = right

while left <= right:
    mid = left + (right - left) / 2
    if canFinish(piles, mid, h):
        result = mid
        right = mid - 1
    else:
        left = mid + 1
return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int left = 1;
        int right = 0;
        for (int pile : piles) {
            right = Math.max(right, pile);
        }
        
        int result = right;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (canFinish(piles, mid, h)) {
                result = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        
        return result;
    }
    
    private boolean canFinish(int[] piles, int speed, int h) {
        int hours = 0;
        for (int pile : piles) {
            hours += (pile + speed - 1) / speed; // Ceiling division
        }
        return hours <= h;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int left = 1;
        int right = *max_element(piles.begin(), piles.end());
        int result = right;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (canFinish(piles, mid, h)) {
                result = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        
        return result;
    }
    
private:
    bool canFinish(vector<int>& piles, int speed, int h) {
        int hours = 0;
        for (int pile : piles) {
            hours += (pile + speed - 1) / speed; // Ceiling division
        }
        return hours <= h;
    }
};
```

</div>

**Time Complexity:** O(n * log(max(piles)))  
**Space Complexity:** O(1)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Linear Search | O(max(piles) * n) | O(1) | Use only when max(piles) is very small |
| Binary Search (Optimal) | O(n * log(max(piles))) | O(1) | Most efficient. Reduces search space logarithmically. Optimal solution |

