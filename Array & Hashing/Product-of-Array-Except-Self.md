# Product of Array Except Self

**Difficulty:** Medium  

## Problem Statement

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operator.

**Example 1:**
```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

**Example 2:**
```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

## Brief Explanation

We need to compute the product of all elements except the current one. The challenge is to do this in O(n) time without division, and handle edge cases like zeros and negative numbers.

## Approach 1: Brute Force

### Explanation
For each element, calculate the product of all other elements by iterating through the array and multiplying all elements except the current one.

### Pseudocode
```
result = new array of size n
for i = 0 to n-1:
    product = 1
    for j = 0 to n-1:
        if i != j:
            product *= nums[j]
    result[i] = product
return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        for (int i = 0; i &lt; n; i++) {
            int product = 1;
            for (int j = 0; j &lt; n; j++) {
                if (i != j) {
                    product *= nums[j];
                }
            }
            result[i] = product;
        }
        return result;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;vector&gt;
class Solution {
public:
    vector&lt;int&gt; productExceptSelf(vector&lt;int&gt;&amp; nums) {
        int n = nums.size();
        vector&lt;int&gt; result(n);
        for (int i = 0; i &lt; n; i++) {
            int product = 1;
            for (int j = 0; j &lt; n; j++) {
                if (i != j) {
                    product *= nums[j];
                }
            }
            result[i] = product;
        }
        return result;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n²)  
**Space Complexity:** O(1) excluding output array
## Approach 2: Left and Right Product Arrays
### Explanation
Create two arrays: one storing products of all elements to the left, and another storing products of all elements to the right. Then multiply corresponding elements from both arrays.
### Pseudocode
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">
n = length(nums)
left = new array of size n
right = new array of size n
result = new array of size n

left[0] = 1
for i = 1 to n-1:
    left[i] = left[i-1] * nums[i-1]

right[n-1] = 1
for i = n-2 down to 0:
    right[i] = right[i+1] * nums[i+1]

for i = 0 to n-1:
    result[i] = left[i] * right[i]

return result
</code></pre>
### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        int[] result = new int[n];
        left[0] = 1;
        for (int i = 1; i &lt; n; i++) {
            left[i] = left[i - 1] * nums[i - 1];
        }
        right[n - 1] = 1;
        for (int i = n - 2; i &gt;= 0; i--) {
            right[i] = right[i + 1] * nums[i + 1];
        }
        for (int i = 0; i &lt; n; i++) {
            result[i] = left[i] * right[i];
        }
        return result;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;vector&gt;
class Solution {
public:
    vector&lt;int&gt; productExceptSelf(vector&lt;int&gt;&amp; nums) {
        int n = nums.size();
        vector&lt;int&gt; left(n);
        vector&lt;int&gt; right(n);
        vector&lt;int&gt; result(n);
        left[0] = 1;
        for (int i = 1; i &lt; n; i++) {
            left[i] = left[i - 1] * nums[i - 1];
        }
        right[n - 1] = 1;
        for (int i = n - 2; i &gt;= 0; i--) {
            right[i] = right[i + 1] * nums[i + 1];
        }
        for (int i = 0; i &lt; n; i++) {
            result[i] = left[i] * right[i];
        }
        return result;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n)  
**Space Complexity:** O(n)
## Approach 3: Space Optimized (Optimal)
### Explanation
Use the output array to store left products first, then multiply with right products in a second pass. This eliminates the need for separate left and right arrays.
### Pseudocode
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">
n = length(nums)
result = new array of size n

result[0] = 1
for i = 1 to n-1:
    result[i] = result[i-1] * nums[i-1]

right = 1
for i = n-1 down to 0:
    result[i] *= right
    right *= nums[i]

return result
</code></pre>
### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        result[0] = 1;
        for (int i = 1; i &lt; n; i++) {
            result[i] = result[i - 1] * nums[i - 1];
        }
        int right = 1;
        for (int i = n - 1; i &gt;= 0; i--) {
            result[i] *= right;
            right *= nums[i];
        }
        return result;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;vector&gt;
class Solution {
public:
    vector&lt;int&gt; productExceptSelf(vector&lt;int&gt;&amp; nums) {
        int n = nums.size();
        vector&lt;int&gt; result(n);
        result[0] = 1;
        for (int i = 1; i &lt; n; i++) {
            result[i] = result[i - 1] * nums[i - 1];
        }
        int right = 1;
        for (int i = n - 1; i &gt;= 0; i--) {
            result[i] *= right;
            right *= nums[i];
        }
        return result;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n)  
**Space Complexity:** O(1) excluding output array
## Comparison Table
| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n²) | O(1) excluding output | Use only for very small arrays or educational purposes |
| Left and Right Product Arrays | O(n) | O(n) | Clear and efficient. Good for understanding the concept. Use when clarity is more important than space optimization |
| Space Optimized (Optimal) | O(n) | O(1) excluding output | Most efficient solution. Best approach for production code and interviews. Use when space optimization is important |