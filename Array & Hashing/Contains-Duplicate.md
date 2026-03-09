# Contains Duplicate

**Difficulty:** Easy

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
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">import java.util.Arrays;
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0; i &lt; nums.length - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;algorithm&gt;
#include &lt;vector&gt;
class Solution {
public:
    bool containsDuplicate(vector&lt;int&gt;&amp; nums) {
        sort(nums.begin(), nums.end());
        for (int i = 0; i &lt; nums.size() - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n log n)  
**Space Complexity:** O(1)
## Approach 3: Hash Set (Optimal)
### Explanation
Use a hash set to store elements as we iterate. If we encounter an element that already exists in the set, we found a duplicate.
### Pseudocode
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">
set = new HashSet()
for each num in nums:
    if num in set:
        return true
    add num to set
return false
</code></pre>
### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">import java.util.HashSet;
import java.util.Set;
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set&lt;Integer&gt; seen = new HashSet&lt;&gt;();
        for (int num : nums) {
            if (seen.contains(num)) {
                return true;
            }
            seen.add(num);
        }
        return false;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;unordered_set&gt;
#include &lt;vector&gt;
class Solution {
public:
    bool containsDuplicate(vector&lt;int&gt;&amp; nums) {
        unordered_set&lt;int&gt; seen;
        for (int num : nums) {
            if (seen.find(num) != seen.end()) {
                return true;
            }
            seen.insert(num);
        }
        return false;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n)  
**Space Complexity:** O(n)
## Comparison Table
| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n²) | O(1) | Only for very small arrays |
| Sorting | O(n log n) | O(1) | When you need the array sorted anyway, or when memory is a concern |
| Hash Set (Optimal) | O(n) | O(n) | Best for most cases. Use when time is more important than space |