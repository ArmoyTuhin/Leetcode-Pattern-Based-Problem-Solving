# Contains Duplicate

**Difficulty:** Easy  
**Status:** Solved  
**Star:** No

## Problem Statement

Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

**Example 1:**
```
Input: nums = [1,2,3,1]
Output: true
```

**Example 2:**
```
Input: nums = [1,2,3,4]
Output: false
```

**Example 3:**
```
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
```

## Brief Explanation

The challenge is to determine if there are any duplicate elements in the array. The main challenge is to do this efficiently without using excessive memory or time.

## Approach 1: Brute Force

### Explanation
Compare each element with every other element in the array. If any two elements are equal, return true.

### Pseudocode
```
for i = 0 to n-1:
    for j = i+1 to n-1:
        if nums[i] == nums[j]:
            return true
return false
```

### Java Code
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] == nums[j]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

### C++ Code
```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] == nums[j]) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

**Time Complexity:** O(n²)  
**Space Complexity:** O(1)

## Approach 2: Sorting

### Explanation
Sort the array first, then check adjacent elements. If any adjacent elements are equal, there's a duplicate.

### Pseudocode
```
sort(nums)
for i = 0 to n-2:
    if nums[i] == nums[i+1]:
        return true
return false
```

### Java Code
```java
import java.util.Arrays;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
}
```

### C++ Code
```cpp
#include <algorithm>
#include <vector>

class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size() - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
};
```

**Time Complexity:** O(n log n)  
**Space Complexity:** O(1)

## Approach 3: Hash Set (Optimal)

### Explanation
Use a hash set to store elements as we iterate. If we encounter an element that already exists in the set, we found a duplicate.

### Pseudocode
```
set = new HashSet()
for each num in nums:
    if num in set:
        return true
    add num to set
return false
```

### Java Code
```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> seen = new HashSet<>();
        for (int num : nums) {
            if (seen.contains(num)) {
                return true;
            }
            seen.add(num);
        }
        return false;
    }
}
```

### C++ Code
```cpp
#include <unordered_set>
#include <vector>

class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> seen;
        for (int num : nums) {
            if (seen.find(num) != seen.end()) {
                return true;
            }
            seen.insert(num);
        }
        return false;
    }
};
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

## Comparison

- **Brute Force:** Use only when the array is very small. Time complexity is O(n²) which is inefficient for large arrays.
- **Sorting:** Good when you need the array sorted anyway, or when memory is a concern. Time complexity is O(n log n).
- **Hash Set (Optimal):** Best for most cases. Linear time complexity O(n) with O(n) space. Use when time is more important than space.

