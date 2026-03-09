# Two Sum

**Difficulty:** Easy  

## Problem Statement

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

**Example 1:**
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**
```
Input: nums = [3,3], target = 6
Output: [0,1]
```

## Brief Explanation

We need to find two numbers in the array that sum to the target value and return their indices. The challenge is to do this efficiently without checking all pairs.

## Approach 1: Brute Force

### Explanation
Check all possible pairs of numbers in the array. For each pair, check if their sum equals the target.

### Pseudocode
```
for i = 0 to n-1:
    for j = i+1 to n-1:
        if nums[i] + nums[j] == target:
            return [i, j]
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
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
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
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

## Approach 2: Two-Pass Hash Map

### Explanation
First pass: store each number and its index in a hash map. Second pass: for each number, check if the complement (target - number) exists in the map.

### Pseudocode
```
map = new HashMap()
for i = 0 to n-1:
    map.put(nums[i], i)

for i = 0 to n-1:
    complement = target - nums[i]
    if complement in map and map[complement] != i:
        return [i, map[complement]]
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], i);
        }
        
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement) && map.get(complement) != i) {
                return new int[]{i, map.get(complement)};
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
#include <unordered_map>

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        
        for (int i = 0; i < nums.size(); i++) {
            map[nums[i]] = i;
        }
        
        for (int i = 0; i < nums.size(); i++) {
            int complement = target - nums[i];
            if (map.find(complement) != map.end() && map[complement] != i) {
                return {i, map[complement]};
            }
        }
        return {};
    }
};

```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

## Approach 3: One-Pass Hash Map (Optimal)

### Explanation
Iterate through the array once. For each number, check if its complement exists in the map. If not, add the current number to the map. This way we can find the pair in a single pass.

### Pseudocode
```
map = new HashMap()
for i = 0 to n-1:
    complement = target - nums[i]
    if complement in map:
        return [map[complement], i]
    map.put(nums[i], i)
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }
            map.put(nums[i], i);
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
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        
        for (int i = 0; i < nums.size(); i++) {
            int complement = target - nums[i];
            if (map.find(complement) != map.end()) {
                return {map[complement], i};
            }
            map[nums[i]] = i;
        }
        return {};
    }
};

```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n²) | O(1) | Use only for very small arrays or when memory is extremely limited |
| Two-Pass Hash Map | O(n) | O(n) | Good for understanding but requires two iterations. Slightly less efficient than one-pass |
| One-Pass Hash Map (Optimal) | O(n) | O(n) | Most efficient solution. Single pass through the array. Best approach for this problem |

