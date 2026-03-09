# Trapping Rain Water

**Difficulty:** Hard

## Problem Statement

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**
```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

**Example 2:**
```
Input: height = [4,2,0,3,2,5]
Output: 9
```

## Brief Explanation

We need to calculate how much water can be trapped between bars. Water is trapped when there are bars on both sides that are taller than the current position.

## Approach 1: Brute Force

### Explanation
For each position, find the maximum height on the left and right. The water trapped at that position is the minimum of these two minus the current height.

### Pseudocode
```
water = 0
for i = 0 to n-1:
    leftMax = 0
    rightMax = 0
    
    for j = 0 to i:
        leftMax = max(leftMax, height[j])
    
    for j = i to n-1:
        rightMax = max(rightMax, height[j])
    
    water += min(leftMax, rightMax) - height[i]
return water
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int trap(int[] height) {
        int water = 0;
        int n = height.length;
        
        for (int i = 0; i < n; i++) {
            int leftMax = 0;
            int rightMax = 0;
            
            for (int j = 0; j <= i; j++) {
                leftMax = Math.max(leftMax, height[j]);
            }
            
            for (int j = i; j < n; j++) {
                rightMax = Math.max(rightMax, height[j]);
            }
            
            water += Math.min(leftMax, rightMax) - height[i];
        }
        
        return water;
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
    int trap(vector<int>& height) {
        int water = 0;
        int n = height.size();
        
        for (int i = 0; i < n; i++) {
            int leftMax = 0;
            int rightMax = 0;
            
            for (int j = 0; j <= i; j++) {
                leftMax = max(leftMax, height[j]);
            }
            
            for (int j = i; j < n; j++) {
                rightMax = max(rightMax, height[j]);
            }
            
            water += min(leftMax, rightMax) - height[i];
        }
        
        return water;
    }
};
```

</div>

**Time Complexity:** O(n²)  
**Space Complexity:** O(1)

## Approach 2: Dynamic Programming

### Explanation
Precompute left and right maximum heights for each position using arrays, then calculate trapped water.

### Pseudocode
```
n = length(height)
leftMax = new array of size n
rightMax = new array of size n

leftMax[0] = height[0]
for i = 1 to n-1:
    leftMax[i] = max(leftMax[i-1], height[i])

rightMax[n-1] = height[n-1]
for i = n-2 down to 0:
    rightMax[i] = max(rightMax[i+1], height[i])

water = 0
for i = 0 to n-1:
    water += min(leftMax[i], rightMax[i]) - height[i]
return water
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int[] leftMax = new int[n];
        int[] rightMax = new int[n];
        
        leftMax[0] = height[0];
        for (int i = 1; i < n; i++) {
            leftMax[i] = Math.max(leftMax[i - 1], height[i]);
        }
        
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            rightMax[i] = Math.max(rightMax[i + 1], height[i]);
        }
        
        int water = 0;
        for (int i = 0; i < n; i++) {
            water += Math.min(leftMax[i], rightMax[i]) - height[i];
        }
        
        return water;
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
    int trap(vector<int>& height) {
        int n = height.size();
        vector<int> leftMax(n);
        vector<int> rightMax(n);
        
        leftMax[0] = height[0];
        for (int i = 1; i < n; i++) {
            leftMax[i] = max(leftMax[i - 1], height[i]);
        }
        
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            rightMax[i] = max(rightMax[i + 1], height[i]);
        }
        
        int water = 0;
        for (int i = 0; i < n; i++) {
            water += min(leftMax[i], rightMax[i]) - height[i];
        }
        
        return water;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

## Approach 3: Two Pointers (Optimal)

### Explanation
Use two pointers from both ends. Track the maximum height from left and right. Move the pointer with the smaller maximum height, as the water level is determined by the smaller side.

### Pseudocode
```
left = 0
right = n - 1
leftMax = 0
rightMax = 0
water = 0

while left < right:
    if height[left] < height[right]:
        if height[left] >= leftMax:
            leftMax = height[left]
        else:
            water += leftMax - height[left]
        left++
    else:
        if height[right] >= rightMax:
            rightMax = height[right]
        else:
            water += rightMax - height[right]
        right--
return water
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int trap(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int leftMax = 0;
        int rightMax = 0;
        int water = 0;
        
        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    water += leftMax - height[left];
                }
                left++;
            } else {
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    water += rightMax - height[right];
                }
                right--;
            }
        }
        
        return water;
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
    int trap(vector<int>& height) {
        int left = 0;
        int right = height.size() - 1;
        int leftMax = 0;
        int rightMax = 0;
        int water = 0;
        
        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    water += leftMax - height[left];
                }
                left++;
            } else {
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    water += rightMax - height[right];
                }
                right--;
            }
        }
        
        return water;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n²) | O(1) | Use only for very small arrays |
| Dynamic Programming | O(n) | O(n) | Good for understanding. Uses extra space for precomputation |
| Two Pointers (Optimal) | O(n) | O(1) | Most efficient. Uses constant space. Optimal solution |

