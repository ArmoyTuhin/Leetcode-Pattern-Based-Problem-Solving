# Spiral Matrix

**Difficulty:** Medium

## Problem Statement

Given an `m x n` matrix, return all elements of the matrix in spiral order.

**Example 1:**
```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
```

**Example 2:**
```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

## Brief Explanation

We need to traverse the matrix in spiral order: right, down, left, up. Use boundaries to track the current spiral layer.

## Approach 1: Simulation (Optimal)

### Explanation
Simulate the spiral traversal by maintaining boundaries (top, bottom, left, right) and moving in the four directions.

### Pseudocode
```
result = []
top = 0, bottom = m - 1
left = 0, right = n - 1

while top <= bottom and left <= right:
    // Move right
    for j = left to right:
        result.add(matrix[top][j])
    top++
    
    // Move down
    for i = top to bottom:
        result.add(matrix[i][right])
    right--
    
    if top <= bottom:
        // Move left
        for j = right down to left:
            result.add(matrix[bottom][j])
        bottom--
    
    if left <= right:
        // Move up
        for i = bottom down to top:
            result.add(matrix[i][left])
        left++
return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();
        int m = matrix.length;
        int n = matrix[0].length;
        int top = 0, bottom = m - 1;
        int left = 0, right = n - 1;
        
        while (top <= bottom && left <= right) {
            // Move right
            for (int j = left; j <= right; j++) {
                result.add(matrix[top][j]);
            }
            top++;
            
            // Move down
            for (int i = top; i <= bottom; i++) {
                result.add(matrix[i][right]);
            }
            right--;
            
            if (top <= bottom) {
                // Move left
                for (int j = right; j >= left; j--) {
                    result.add(matrix[bottom][j]);
                }
                bottom--;
            }
            
            if (left <= right) {
                // Move up
                for (int i = bottom; i >= top; i--) {
                    result.add(matrix[i][left]);
                }
                left++;
            }
        }
        
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
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;
        int m = matrix.size();
        int n = matrix[0].size();
        int top = 0, bottom = m - 1;
        int left = 0, right = n - 1;
        
        while (top <= bottom && left <= right) {
            // Move right
            for (int j = left; j <= right; j++) {
                result.push_back(matrix[top][j]);
            }
            top++;
            
            // Move down
            for (int i = top; i <= bottom; i++) {
                result.push_back(matrix[i][right]);
            }
            right--;
            
            if (top <= bottom) {
                // Move left
                for (int j = right; j >= left; j--) {
                    result.push_back(matrix[bottom][j]);
                }
                bottom--;
            }
            
            if (left <= right) {
                // Move up
                for (int i = bottom; i >= top; i--) {
                    result.push_back(matrix[i][left]);
                }
                left++;
            }
        }
        
        return result;
    }
};
```

</div>

**Time Complexity:** O(m * n)  
**Space Complexity:** O(1) excluding output

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Simulation (Optimal) | O(m * n) | O(1) | Most efficient. Direct simulation of spiral traversal. Optimal solution |

