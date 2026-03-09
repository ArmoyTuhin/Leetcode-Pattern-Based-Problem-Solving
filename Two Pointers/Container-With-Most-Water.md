# Container With Most Water

**Difficulty:** Medium

## Problem Statement

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `i-th` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

**Example 1:**
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

**Example 2:**
```
Input: height = [1,1]
Output: 1
```

## Brief Explanation

We need to find two lines that form a container with maximum area. The area is determined by the width (distance between indices) and the height (minimum of the two line heights).

## Approach 1: Brute Force

### Explanation
Check all possible pairs of lines and calculate the area for each pair, keeping track of the maximum.

### Pseudocode
```
maxArea = 0
for i = 0 to n-1:
    for j = i+1 to n-1:
        width = j - i
        height = min(height[i], height[j])
        area = width * height
        maxArea = max(maxArea, area)
return maxArea
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int maxArea(int[] height) {
        int maxArea = 0;
        int n = height.length;
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int width = j - i;
                int minHeight = Math.min(height[i], height[j]);
                int area = width * minHeight;
                maxArea = Math.max(maxArea, area);
            }
        }
        
        return maxArea;
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
    int maxArea(vector<int>& height) {
        int maxArea = 0;
        int n = height.size();
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int width = j - i;
                int minHeight = min(height[i], height[j]);
                int area = width * minHeight;
                maxArea = max(maxArea, area);
            }
        }
        
        return maxArea;
    }
};
```

</div>

**Time Complexity:** O(n²)  
**Space Complexity:** O(1)

## Approach 2: Two Pointers (Optimal)

### Explanation
Start with two pointers at both ends. The area is limited by the shorter line. Move the pointer with the shorter line inward, as moving the taller line inward can only decrease the area.

### Pseudocode
```
left = 0
right = n - 1
maxArea = 0

while left < right:
    width = right - left
    minHeight = min(height[left], height[right])
    area = width * minHeight
    maxArea = max(maxArea, area)
    
    if height[left] < height[right]:
        left++
    else:
        right--
return maxArea
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int maxArea = 0;
        
        while (left < right) {
            int width = right - left;
            int minHeight = Math.min(height[left], height[right]);
            int area = width * minHeight;
            maxArea = Math.max(maxArea, area);
            
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        
        return maxArea;
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
    int maxArea(vector<int>& height) {
        int left = 0;
        int right = height.size() - 1;
        int maxArea = 0;
        
        while (left < right) {
            int width = right - left;
            int minHeight = min(height[left], height[right]);
            int area = width * minHeight;
            maxArea = max(maxArea, area);
            
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        
        return maxArea;
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
| Two Pointers (Optimal) | O(n) | O(1) | Most efficient. Single pass through array. Optimal solution |

