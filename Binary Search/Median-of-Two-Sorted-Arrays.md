# Median of Two Sorted Arrays

**Difficulty:** Hard

## Problem Statement

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the median of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**
```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

**Example 2:**
```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

## Brief Explanation

We need to find the median of two sorted arrays without merging them. Use binary search to partition both arrays such that elements on left are smaller than elements on right.

## Approach 1: Merge and Find

### Explanation
Merge both arrays into one sorted array, then find the median.

### Pseudocode
```
merged = merge(nums1, nums2)
n = merged.length
if n is even:
    return (merged[n/2-1] + merged[n/2]) / 2.0
else:
    return merged[n/2]
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int[] merged = new int[m + n];
        
        int i = 0, j = 0, k = 0;
        while (i < m && j < n) {
            if (nums1[i] <= nums2[j]) {
                merged[k++] = nums1[i++];
            } else {
                merged[k++] = nums2[j++];
            }
        }
        while (i < m) {
            merged[k++] = nums1[i++];
        }
        while (j < n) {
            merged[k++] = nums2[j++];
        }
        
        int len = merged.length;
        if (len % 2 == 0) {
            return (merged[len/2 - 1] + merged[len/2]) / 2.0;
        } else {
            return merged[len/2];
        }
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
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();
        vector<int> merged(m + n);
        
        int i = 0, j = 0, k = 0;
        while (i < m && j < n) {
            if (nums1[i] <= nums2[j]) {
                merged[k++] = nums1[i++];
            } else {
                merged[k++] = nums2[j++];
            }
        }
        while (i < m) {
            merged[k++] = nums1[i++];
        }
        while (j < n) {
            merged[k++] = nums2[j++];
        }
        
        int len = merged.size();
        if (len % 2 == 0) {
            return (merged[len/2 - 1] + merged[len/2]) / 2.0;
        } else {
            return merged[len/2];
        }
    }
};
```

</div>

**Time Complexity:** O(m + n)  
**Space Complexity:** O(m + n)

## Approach 2: Binary Search (Optimal)

### Explanation
Use binary search on the smaller array to find the correct partition. The partition divides both arrays such that left half has (m+n+1)/2 elements.

### Pseudocode
```
if nums1.length > nums2.length:
    swap(nums1, nums2)
    
m = nums1.length
n = nums2.length
left = 0
right = m

while left <= right:
    partition1 = left + (right - left) / 2
    partition2 = (m + n + 1) / 2 - partition1
    
    maxLeft1 = partition1 == 0 ? INT_MIN : nums1[partition1-1]
    minRight1 = partition1 == m ? INT_MAX : nums1[partition1]
    maxLeft2 = partition2 == 0 ? INT_MIN : nums2[partition2-1]
    minRight2 = partition2 == n ? INT_MAX : nums2[partition2]
    
    if maxLeft1 <= minRight2 and maxLeft2 <= minRight1:
        if (m + n) % 2 == 0:
            return (max(maxLeft1, maxLeft2) + min(minRight1, minRight2)) / 2.0
        else:
            return max(maxLeft1, maxLeft2)
    else if maxLeft1 > minRight2:
        right = partition1 - 1
    else:
        left = partition1 + 1
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }
        
        int m = nums1.length;
        int n = nums2.length;
        int left = 0;
        int right = m;
        
        while (left <= right) {
            int partition1 = left + (right - left) / 2;
            int partition2 = (m + n + 1) / 2 - partition1;
            
            int maxLeft1 = (partition1 == 0) ? Integer.MIN_VALUE : nums1[partition1 - 1];
            int minRight1 = (partition1 == m) ? Integer.MAX_VALUE : nums1[partition1];
            int maxLeft2 = (partition2 == 0) ? Integer.MIN_VALUE : nums2[partition2 - 1];
            int minRight2 = (partition2 == n) ? Integer.MAX_VALUE : nums2[partition2];
            
            if (maxLeft1 <= minRight2 && maxLeft2 <= minRight1) {
                if ((m + n) % 2 == 0) {
                    return (Math.max(maxLeft1, maxLeft2) + Math.min(minRight1, minRight2)) / 2.0;
                } else {
                    return Math.max(maxLeft1, maxLeft2);
                }
            } else if (maxLeft1 > minRight2) {
                right = partition1 - 1;
            } else {
                left = partition1 + 1;
            }
        }
        
        return 0.0;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <algorithm>
#include <climits>

class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.size() > nums2.size()) {
            return findMedianSortedArrays(nums2, nums1);
        }
        
        int m = nums1.size();
        int n = nums2.size();
        int left = 0;
        int right = m;
        
        while (left <= right) {
            int partition1 = left + (right - left) / 2;
            int partition2 = (m + n + 1) / 2 - partition1;
            
            int maxLeft1 = (partition1 == 0) ? INT_MIN : nums1[partition1 - 1];
            int minRight1 = (partition1 == m) ? INT_MAX : nums1[partition1];
            int maxLeft2 = (partition2 == 0) ? INT_MIN : nums2[partition2 - 1];
            int minRight2 = (partition2 == n) ? INT_MAX : nums2[partition2];
            
            if (maxLeft1 <= minRight2 && maxLeft2 <= minRight1) {
                if ((m + n) % 2 == 0) {
                    return (max(maxLeft1, maxLeft2) + min(minRight1, minRight2)) / 2.0;
                } else {
                    return max(maxLeft1, maxLeft2);
                }
            } else if (maxLeft1 > minRight2) {
                right = partition1 - 1;
            } else {
                left = partition1 + 1;
            }
        }
        
        return 0.0;
    }
};
```

</div>

**Time Complexity:** O(log(min(m, n)))  
**Space Complexity:** O(1)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Merge and Find | O(m + n) | O(m + n) | Simple to understand but doesn't meet O(log(m+n)) requirement |
| Binary Search (Optimal) | O(log(min(m, n))) | O(1) | Most efficient. Meets O(log(m+n)) requirement. Optimal solution |

