# Longest Consecutive Sequence

**Difficulty:** Hard  

## Problem Statement

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

**Example 1:**
```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

**Example 2:**
```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

## Brief Explanation

We need to find the longest sequence of consecutive integers in an unsorted array. The challenge is to achieve O(n) time complexity, which requires avoiding sorting and using efficient data structures.

## Approach 1: Brute Force (Check All Sequences)

### Explanation
For each number, check if it's the start of a sequence by verifying if num-1 exists. If it's a start, count how long the consecutive sequence is.

### Pseudocode
```
maxLength = 0
for each num in nums:
    currentNum = num
    currentLength = 1
    
    while (currentNum + 1) in nums:
        currentNum++
        currentLength++
    
    maxLength = max(maxLength, currentLength)

return maxLength
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) return 0;
        
        Set<Integer> numSet = new HashSet<>();
        for (int num : nums) {
            numSet.add(num);
        }
        
        int maxLength = 0;
        for (int num : nums) {
            int currentNum = num;
            int currentLength = 1;
            
            while (numSet.contains(currentNum + 1)) {
                currentNum++;
                currentLength++;
            }
            
            maxLength = Math.max(maxLength, currentLength);
        }
        
        return maxLength;
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
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        unordered_set<int> numSet(nums.begin(), nums.end());
        
        int maxLength = 0;
        for (int num : nums) {
            int currentNum = num;
            int currentLength = 1;
            
            while (numSet.find(currentNum + 1) != numSet.end()) {
                currentNum++;
                currentLength++;
            }
            
            maxLength = max(maxLength, currentLength);
        }
        
        return maxLength;
    }
};

```

</div>

**Time Complexity:** O(n²) in worst case  
**Space Complexity:** O(n)

## Approach 2: Sorting

### Explanation
Sort the array first, then iterate through to find the longest consecutive sequence. Handle duplicates and track the current sequence length.

### Pseudocode
```
if nums is empty:
    return 0

sort(nums)
maxLength = 1
currentLength = 1

for i = 1 to n-1:
    if nums[i] == nums[i-1]:
        continue
    else if nums[i] == nums[i-1] + 1:
        currentLength++
    else:
        maxLength = max(maxLength, currentLength)
        currentLength = 1

return max(maxLength, currentLength)
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.Arrays;

class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) return 0;
        
        Arrays.sort(nums);
        int maxLength = 1;
        int currentLength = 1;
        
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i - 1]) {
                continue;
            } else if (nums[i] == nums[i - 1] + 1) {
                currentLength++;
            } else {
                maxLength = Math.max(maxLength, currentLength);
                currentLength = 1;
            }
        }
        
        return Math.max(maxLength, currentLength);
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
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        sort(nums.begin(), nums.end());
        int maxLength = 1;
        int currentLength = 1;
        
        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] == nums[i - 1]) {
                continue;
            } else if (nums[i] == nums[i - 1] + 1) {
                currentLength++;
            } else {
                maxLength = max(maxLength, currentLength);
                currentLength = 1;
            }
        }
        
        return max(maxLength, currentLength);
    }
};

```

</div>

**Time Complexity:** O(n log n)  
**Space Complexity:** O(1) excluding input

## Approach 3: Hash Set with Sequence Start Detection (Optimal)

### Explanation
Use a hash set for O(1) lookups. Only start counting sequences from numbers that are the start of a sequence (i.e., num-1 doesn't exist). This ensures each number is visited at most twice.

### Pseudocode
```
if nums is empty:
    return 0

numSet = new HashSet(nums)
maxLength = 0

for each num in numSet:
    if (num - 1) not in numSet:
        currentNum = num
        currentLength = 1
        
        while (currentNum + 1) in numSet:
            currentNum++
            currentLength++
        
        maxLength = max(maxLength, currentLength)

return maxLength
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) return 0;
        
        Set<Integer> numSet = new HashSet<>();
        for (int num : nums) {
            numSet.add(num);
        }
        
        int maxLength = 0;
        for (int num : numSet) {
            if (!numSet.contains(num - 1)) {
                int currentNum = num;
                int currentLength = 1;
                
                while (numSet.contains(currentNum + 1)) {
                    currentNum++;
                    currentLength++;
                }
                
                maxLength = Math.max(maxLength, currentLength);
            }
        }
        
        return maxLength;
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
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        unordered_set<int> numSet(nums.begin(), nums.end());
        
        int maxLength = 0;
        for (int num : numSet) {
            if (numSet.find(num - 1) == numSet.end()) {
                int currentNum = num;
                int currentLength = 1;
                
                while (numSet.find(currentNum + 1) != numSet.end()) {
                    currentNum++;
                    currentLength++;
                }
                
                maxLength = max(maxLength, currentLength);
            }
        }
        
        return maxLength;
    }
};

```

</div>

**Time Complexity:** O(n) - each number is visited at most twice  
**Space Complexity:** O(n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n²) | O(n) | Simple but inefficient. In worst case, for each number we might check all other numbers. Use only for very small arrays |
| Sorting | O(n log n) | O(1) excluding input | Straightforward approach. Easy to understand and implement. Use when O(n log n) is acceptable and you prefer simplicity |
| Hash Set with Sequence Start Detection (Optimal) | O(n) | O(n) | Most efficient. Only processes sequence starts, ensuring each number is visited at most twice. Best approach for the O(n) requirement |

