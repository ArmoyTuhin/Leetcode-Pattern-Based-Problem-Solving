# Sliding Window Maximum

**Difficulty:** Hard

## Problem Statement

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return the maximum element in each sliding window.

**Example 1:**
```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Example 2:**
```
Input: nums = [1], k = 1
Output: [1]
```

## Brief Explanation

We need to find the maximum element in each sliding window of size k. Use a deque (double-ended queue) to maintain indices of elements in decreasing order.

## Approach 1: Brute Force

### Explanation
For each window position, find the maximum element in that window.

### Pseudocode
```
result = []
for i = 0 to n - k:
    maxVal = nums[i]
    for j = i to i + k - 1:
        maxVal = max(maxVal, nums[j])
    result.add(maxVal)
return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] result = new int[n - k + 1];
        
        for (int i = 0; i <= n - k; i++) {
            int maxVal = nums[i];
            for (int j = i; j < i + k; j++) {
                maxVal = Math.max(maxVal, nums[j]);
            }
            result[i] = maxVal;
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
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> result;
        
        for (int i = 0; i <= n - k; i++) {
            int maxVal = nums[i];
            for (int j = i; j < i + k; j++) {
                maxVal = max(maxVal, nums[j]);
            }
            result.push_back(maxVal);
        }
        
        return result;
    }
};
```

</div>

**Time Complexity:** O(n * k)  
**Space Complexity:** O(1) excluding output

## Approach 2: Deque (Optimal)

### Explanation
Use a deque to store indices. Maintain indices in decreasing order of their values. Remove indices outside the window and indices with smaller values.

### Pseudocode
```
deque = new Deque()
result = []

for i = 0 to n-1:
    while deque is not empty and nums[deque.back()] < nums[i]:
        deque.pop_back()
    deque.push_back(i)
    
    if deque.front() < i - k + 1:
        deque.pop_front()
    
    if i >= k - 1:
        result.add(nums[deque.front()])
return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        Deque<Integer> deque = new ArrayDeque<>();
        List<Integer> result = new ArrayList<>();
        
        for (int i = 0; i < nums.length; i++) {
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }
            deque.offerLast(i);
            
            if (deque.peekFirst() < i - k + 1) {
                deque.pollFirst();
            }
            
            if (i >= k - 1) {
                result.add(nums[deque.peekFirst()]);
            }
        }
        
        return result.stream().mapToInt(i -> i).toArray();
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <deque>

class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> dq;
        vector<int> result;
        
        for (int i = 0; i < nums.size(); i++) {
            while (!dq.empty() && nums[dq.back()] < nums[i]) {
                dq.pop_back();
            }
            dq.push_back(i);
            
            if (dq.front() < i - k + 1) {
                dq.pop_front();
            }
            
            if (i >= k - 1) {
                result.push_back(nums[dq.front()]);
            }
        }
        
        return result;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(k)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n * k) | O(1) | Use only when k is very small |
| Deque (Optimal) | O(n) | O(k) | Most efficient. Each element is added and removed at most once. Optimal solution |

