# 3Sum

**Difficulty:** Medium

## Problem Statement

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**
```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = (-1) + 0 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```

**Example 2:**
```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```

**Example 3:**
```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only unique triplet sums up to 0.
```

## Brief Explanation

We need to find all unique triplets that sum to zero. The challenge is avoiding duplicates and doing this efficiently.

## Approach 1: Brute Force

### Explanation
Check all possible triplets and add valid ones to the result, avoiding duplicates.

### Pseudocode
```
result = []
for i = 0 to n-1:
    for j = i+1 to n-1:
        for k = j+1 to n-1:
            if nums[i] + nums[j] + nums[k] == 0:
                triplet = [nums[i], nums[j], nums[k]]
                sort(triplet)
                if triplet not in result:
                    result.add(triplet)
return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> result = new HashSet<>();
        int n = nums.length;
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> triplet = Arrays.asList(nums[i], nums[j], nums[k]);
                        Collections.sort(triplet);
                        result.add(triplet);
                    }
                }
            }
        }
        
        return new ArrayList<>(result);
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <set>
#include <algorithm>

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        set<vector<int>> result;
        int n = nums.size();
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        vector<int> triplet = {nums[i], nums[j], nums[k]};
                        sort(triplet.begin(), triplet.end());
                        result.insert(triplet);
                    }
                }
            }
        }
        
        return vector<vector<int>>(result.begin(), result.end());
    }
};
```

</div>

**Time Complexity:** O(n³)  
**Space Complexity:** O(n)

## Approach 2: Hash Set

### Explanation
For each element, use a hash set to find pairs that sum to the negative of that element.

### Pseudocode
```
sort(nums)
result = []
for i = 0 to n-1:
    if i > 0 and nums[i] == nums[i-1]:
        continue
    target = -nums[i]
    seen = new HashSet()
    for j = i+1 to n-1:
        complement = target - nums[j]
        if complement in seen:
            result.add([nums[i], complement, nums[j]])
        seen.add(nums[j])
return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        int n = nums.length;
        
        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            
            int target = -nums[i];
            Set<Integer> seen = new HashSet<>();
            
            for (int j = i + 1; j < n; j++) {
                int complement = target - nums[j];
                if (seen.contains(complement)) {
                    result.add(Arrays.asList(nums[i], complement, nums[j]));
                    while (j + 1 < n && nums[j] == nums[j + 1]) {
                        j++;
                    }
                }
                seen.add(nums[j]);
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
#include <unordered_set>
#include <algorithm>

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        int n = nums.size();
        
        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            
            int target = -nums[i];
            unordered_set<int> seen;
            
            for (int j = i + 1; j < n; j++) {
                int complement = target - nums[j];
                if (seen.find(complement) != seen.end()) {
                    result.push_back({nums[i], complement, nums[j]});
                    while (j + 1 < n && nums[j] == nums[j + 1]) {
                        j++;
                    }
                }
                seen.insert(nums[j]);
            }
        }
        
        return result;
    }
};
```

</div>

**Time Complexity:** O(n²)  
**Space Complexity:** O(n)

## Approach 3: Two Pointers (Optimal)

### Explanation
Sort the array first. For each element, use two pointers to find pairs that sum to the negative of that element.

### Pseudocode
```
sort(nums)
result = []
for i = 0 to n-1:
    if i > 0 and nums[i] == nums[i-1]:
        continue
    left = i + 1
    right = n - 1
    target = -nums[i]
    
    while left < right:
        sum = nums[left] + nums[right]
        if sum == target:
            result.add([nums[i], nums[left], nums[right]])
            while left < right and nums[left] == nums[left+1]:
                left++
            while left < right and nums[right] == nums[right-1]:
                right--
            left++
            right--
        else if sum < target:
            left++
        else:
            right--
return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        int n = nums.length;
        
        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            
            int left = i + 1;
            int right = n - 1;
            int target = -nums[i];
            
            while (left < right) {
                int sum = nums[left] + nums[right];
                if (sum == target) {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    left++;
                    right--;
                } else if (sum < target) {
                    left++;
                } else {
                    right--;
                }
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
#include <algorithm>

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        int n = nums.size();
        
        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            
            int left = i + 1;
            int right = n - 1;
            int target = -nums[i];
            
            while (left < right) {
                int sum = nums[left] + nums[right];
                if (sum == target) {
                    result.push_back({nums[i], nums[left], nums[right]});
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    left++;
                    right--;
                } else if (sum < target) {
                    left++;
                } else {
                    right--;
                }
            }
        }
        
        return result;
    }
};
```

</div>

**Time Complexity:** O(n²)  
**Space Complexity:** O(1) excluding output

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n³) | O(n) | Use only for very small arrays |
| Hash Set | O(n²) | O(n) | Good alternative but uses extra space |
| Two Pointers (Optimal) | O(n²) | O(1) | Most efficient. Uses constant space. Best for this problem |

