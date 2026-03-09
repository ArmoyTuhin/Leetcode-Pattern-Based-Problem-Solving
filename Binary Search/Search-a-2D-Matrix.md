# Search a 2D Matrix

**Difficulty:** Medium

## Problem Statement

You are given an `m x n` integer matrix `matrix` with the following two properties:
- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` if `target` is in `matrix` or `false` otherwise.

You must write a solution in `O(log(m * n))` time complexity.

**Example 1:**
```
Input: matrix = [[1,4,7,11],[2,5,8,12],[3,6,9,16],[10,13,14,17]], target = 5
Output: true
```

**Example 2:**
```
Input: matrix = [[1,4,7,11],[2,5,8,12],[3,6,9,16],[10,13,14,17]], target = 3
Output: false
```

## Brief Explanation

The matrix can be treated as a sorted 1D array. Use binary search by converting 2D indices to 1D index and vice versa.

## Approach 1: Linear Search

### Explanation
Search through each row and column to find the target.

### Pseudocode
```
for i = 0 to m-1:
    for j = 0 to n-1:
        if matrix[i][j] == target:
            return true
return false
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        int n = matrix[0].length;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == target) {
                    return true;
                }
            }
        }
        return false;
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
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        int n = matrix[0].size();
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == target) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

</div>

**Time Complexity:** O(m * n)  
**Space Complexity:** O(1)

## Approach 2: Binary Search (Optimal)

### Explanation
Treat the 2D matrix as a 1D sorted array. Convert 1D index to 2D coordinates: row = index / n, col = index % n.

### Pseudocode
```
m = matrix.length
n = matrix[0].length
left = 0
right = m * n - 1

while left <= right:
    mid = left + (right - left) / 2
    row = mid / n
    col = mid % n
    value = matrix[row][col]
    
    if value == target:
        return true
    else if value < target:
        left = mid + 1
    else:
        right = mid - 1
return false
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        int n = matrix[0].length;
        int left = 0;
        int right = m * n - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int row = mid / n;
            int col = mid % n;
            int value = matrix[row][col];
            
            if (value == target) {
                return true;
            } else if (value < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return false;
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
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        int n = matrix[0].size();
        int left = 0;
        int right = m * n - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int row = mid / n;
            int col = mid % n;
            int value = matrix[row][col];
            
            if (value == target) {
                return true;
            } else if (value < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return false;
    }
};
```

</div>

**Time Complexity:** O(log(m * n))  
**Space Complexity:** O(1)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Linear Search | O(m * n) | O(1) | Use only for very small matrices |
| Binary Search (Optimal) | O(log(m * n)) | O(1) | Most efficient. Meets O(log(m * n)) requirement. Optimal solution |

