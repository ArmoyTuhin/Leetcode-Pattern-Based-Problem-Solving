# Largest Rectangle In Histogram

**Difficulty:** Hard

## Problem Statement

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return the area of the largest rectangle in the histogram.

**Example 1:**
```
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.
```

**Example 2:**
```
Input: heights = [2,4]
Output: 4
```

## Brief Explanation

We need to find the largest rectangle that can be formed from the histogram bars. The key is finding the maximum area where each bar can extend to form a rectangle.

## Approach 1: Brute Force

### Explanation
For each bar, find how far it can extend to the left and right while maintaining its height, then calculate the area.

### Pseudocode
```
maxArea = 0
for i = 0 to n-1:
    left = i
    right = i
    
    while left >= 0 and heights[left] >= heights[i]:
        left--
    while right < n and heights[right] >= heights[i]:
        right++
    
    width = right - left - 1
    area = heights[i] * width
    maxArea = max(maxArea, area)
return maxArea
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int maxArea = 0;
        int n = heights.length;
        
        for (int i = 0; i < n; i++) {
            int left = i;
            int right = i;
            
            while (left >= 0 && heights[left] >= heights[i]) {
                left--;
            }
            while (right < n && heights[right] >= heights[i]) {
                right++;
            }
            
            int width = right - left - 1;
            int area = heights[i] * width;
            maxArea = Math.max(maxArea, area);
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
    int largestRectangleArea(vector<int>& heights) {
        int maxArea = 0;
        int n = heights.size();
        
        for (int i = 0; i < n; i++) {
            int left = i;
            int right = i;
            
            while (left >= 0 && heights[left] >= heights[i]) {
                left--;
            }
            while (right < n && heights[right] >= heights[i]) {
                right++;
            }
            
            int width = right - left - 1;
            int area = heights[i] * width;
            maxArea = max(maxArea, area);
        }
        
        return maxArea;
    }
};
```

</div>

**Time Complexity:** O(n²)  
**Space Complexity:** O(1)

## Approach 2: Monotonic Stack (Optimal)

### Explanation
Use a stack to track indices. For each bar, pop bars from stack that are taller, calculate area using popped bar as height and current position as right boundary.

### Pseudocode
```
stack = new Stack()
maxArea = 0

for i = 0 to n:
    while stack is not empty and (i == n or heights[i] < heights[stack.top()]):
        height = heights[stack.pop()]
        width = stack.isEmpty() ? i : i - stack.top() - 1
        area = height * width
        maxArea = max(maxArea, area)
    stack.push(i)
return maxArea
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.Stack;

class Solution {
    public int largestRectangleArea(int[] heights) {
        Stack<Integer> stack = new Stack<>();
        int maxArea = 0;
        int n = heights.length;
        
        for (int i = 0; i <= n; i++) {
            int currentHeight = (i == n) ? 0 : heights[i];
            
            while (!stack.isEmpty() && currentHeight < heights[stack.peek()]) {
                int height = heights[stack.pop()];
                int width = stack.isEmpty() ? i : i - stack.peek() - 1;
                int area = height * width;
                maxArea = Math.max(maxArea, area);
            }
            stack.push(i);
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
#include <stack>
#include <algorithm>

class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> st;
        int maxArea = 0;
        int n = heights.size();
        
        for (int i = 0; i <= n; i++) {
            int currentHeight = (i == n) ? 0 : heights[i];
            
            while (!st.empty() && currentHeight < heights[st.top()]) {
                int height = heights[st.top()];
                st.pop();
                int width = st.empty() ? i : i - st.top() - 1;
                int area = height * width;
                maxArea = max(maxArea, area);
            }
            st.push(i);
        }
        
        return maxArea;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n²) | O(1) | Use only for very small arrays |
| Monotonic Stack (Optimal) | O(n) | O(n) | Most efficient. Each bar is pushed and popped once. Optimal solution |

