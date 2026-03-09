# Find Minimum In Rotated Sorted Array

**Difficulty:** Medium

## Problem Statement

Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:
- `[4,5,6,7,0,1,2]` if it was rotated `4` times.
- `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.

You must write an algorithm that runs in `O(log n)` time.

**Example 1:**
```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

**Example 2:**
```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

**Example 3:**
```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The array was rotated 0 times (i.e. no rotation).
```

## Brief Explanation

The minimum element is at the pivot point where rotation occurred. Use binary search to find this pivot by comparing middle element with the rightmost element.

## Approach 1: Linear Search

### Explanation
Iterate through the array to find the minimum element.

### Pseudocode
```
min = nums[0]
for i = 1 to n-1:
    if nums[i] < min:
        min = nums[i]
return min
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int findMin(int[] nums) {
        int min = nums[0];
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] < min) {
                min = nums[i];
            }
        }
        return min;
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
    int findMin(vector<int>& nums) {
        int min = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] < min) {
                min = nums[i];
            }
        }
        return min;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

## Approach 2: Binary Search (Optimal)

### Explanation
Use binary search. Compare middle element with rightmost element to determine which half contains the minimum.

### Pseudocode
```
left = 0
right = n - 1

while left < right:
    mid = left + (right - left) / 2
    if nums[mid] > nums[right]:
        left = mid + 1
    else:
        right = mid
return nums[left]
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return nums[left];
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
    int findMin(vector<int>& nums) {
        int left = 0;
        int right = nums.size() - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return nums[left];
    }
};
```

</div>

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Linear Search | O(n) | O(1) | Use only when array is very small |
| Binary Search (Optimal) | O(log n) | O(1) | Most efficient. Meets O(log n) requirement. Optimal solution |

