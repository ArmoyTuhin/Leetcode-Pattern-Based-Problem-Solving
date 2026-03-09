# Two Sum II - Input Array Is Sorted

**Difficulty:** Medium

## Problem Statement

Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return the indices of the two numbers, `index1` and `index2`, added by one as an integer array `[index1, index2]` of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

**Example 1:**
```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
```

**Example 2:**
```
Input: numbers = [2,3,4], target = 6
Output: [1,3]
Explanation: The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].
```

**Example 3:**
```
Input: numbers = [-1,0], target = -1
Output: [1,2]
Explanation: The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].
```

## Brief Explanation

Since the array is sorted, we can use two pointers to find the two numbers that sum to target. Start with one pointer at the beginning and one at the end, and move them based on the current sum.

## Approach 1: Brute Force

### Explanation
Check all possible pairs of numbers to find the pair that sums to target.

### Pseudocode
```
for i = 0 to n-1:
    for j = i+1 to n-1:
        if numbers[i] + numbers[j] == target:
            return [i+1, j+1]
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int n = numbers.length;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (numbers[i] + numbers[j] == target) {
                    return new int[]{i + 1, j + 1};
                }
            }
        }
        return new int[]{};
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
    vector<int> twoSum(vector<int>& numbers, int target) {
        int n = numbers.size();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (numbers[i] + numbers[j] == target) {
                    return {i + 1, j + 1};
                }
            }
        }
        return {};
    }
};
```

</div>

**Time Complexity:** O(n²)  
**Space Complexity:** O(1)

## Approach 2: Hash Map

### Explanation
Use a hash map to store numbers and their indices. For each number, check if the complement exists in the map.

### Pseudocode
```
map = new HashMap()
for i = 0 to n-1:
    complement = target - numbers[i]
    if complement in map:
        return [map[complement]+1, i+1]
    map[numbers[i]] = i
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] numbers, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < numbers.length; i++) {
            int complement = target - numbers[i];
            if (map.containsKey(complement)) {
                return new int[]{map.get(complement) + 1, i + 1};
            }
            map.put(numbers[i], i);
        }
        return new int[]{};
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <unordered_map>

class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        unordered_map<int, int> map;
        for (int i = 0; i < numbers.size(); i++) {
            int complement = target - numbers[i];
            if (map.find(complement) != map.end()) {
                return {map[complement] + 1, i + 1};
            }
            map[numbers[i]] = i;
        }
        return {};
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

## Approach 3: Two Pointers (Optimal)

### Explanation
Since the array is sorted, use two pointers. If sum is too small, move left pointer right. If sum is too large, move right pointer left.

### Pseudocode
```
left = 0
right = n - 1

while left < right:
    sum = numbers[left] + numbers[right]
    if sum == target:
        return [left+1, right+1]
    else if sum < target:
        left++
    else:
        right--
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;
        
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum == target) {
                return new int[]{left + 1, right + 1};
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        
        return new int[]{};
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
    vector<int> twoSum(vector<int>& numbers, int target) {
        int left = 0;
        int right = numbers.size() - 1;
        
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum == target) {
                return {left + 1, right + 1};
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        
        return {};
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
| Hash Map | O(n) | O(n) | Good when array is not sorted or when you need O(n) time |
| Two Pointers (Optimal) | O(n) | O(1) | Best for sorted arrays. Uses constant space. Optimal solution |

