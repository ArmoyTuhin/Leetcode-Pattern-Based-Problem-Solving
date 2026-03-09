# Top K Frequent Elements

**Difficulty:** Medium  

## Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

**Example 1:**
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**
```
Input: nums = [1], k = 1
Output: [1]
```

## Brief Explanation

We need to find the k most frequent elements in the array. The challenge is to efficiently count frequencies and select the top k elements without sorting the entire frequency map.

## Approach 1: Brute Force (Sort by Frequency)

### Explanation
Count the frequency of each element, then sort all elements by their frequency in descending order, and return the top k elements.

### Pseudocode
```
freqMap = new HashMap()
for each num in nums:
    freqMap[num]++

elements = list of all keys from freqMap
sort elements by frequency in descending order
return first k elements
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">import java.util.*;
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map&lt;Integer, Integer&gt; freqMap = new HashMap&lt;&gt;();
        for (int num : nums) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }
        List&lt;Integer&gt; elements = new ArrayList&lt;&gt;(freqMap.keySet());
        elements.sort((a, b) -&gt; freqMap.get(b) - freqMap.get(a));
        int[] result = new int[k];
        for (int i = 0; i &lt; k; i++) {
            result[i] = elements.get(i);
        }
        return result;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;vector&gt;
#include &lt;unordered_map&gt;
#include &lt;algorithm&gt;
class Solution {
public:
    vector&lt;int&gt; topKFrequent(vector&lt;int&gt;&amp; nums, int k) {
        unordered_map&lt;int, int&gt; freqMap;
        for (int num : nums) {
            freqMap[num]++;
        }
        vector&lt;int&gt; elements;
        for (auto&amp; pair : freqMap) {
            elements.push_back(pair.first);
        }
        sort(elements.begin(), elements.end(), 
             [&amp;freqMap](int a, int b) {
                 return freqMap[a] &gt; freqMap[b];
             });
        return vector&lt;int&gt;(elements.begin(), elements.begin() + k);
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n log n)  
**Space Complexity:** O(n)
## Approach 2: Min Heap (Priority Queue)
### Explanation
Use a min heap of size k to maintain the k most frequent elements. For each element, if the heap size is less than k, add it. Otherwise, if the current element's frequency is greater than the minimum in the heap, replace it.
### Pseudocode
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">
freqMap = new HashMap()
for each num in nums:
    freqMap[num]++

minHeap = new PriorityQueue of size k (by frequency)
for each entry in freqMap:
    if minHeap.size() &lt; k:
        minHeap.add(entry)
    else if entry.frequency &gt; minHeap.peek().frequency:
        minHeap.poll()
        minHeap.add(entry)

return all elements from minHeap
</code></pre>
### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">import java.util.*;
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map&lt;Integer, Integer&gt; freqMap = new HashMap&lt;&gt;();
        for (int num : nums) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }
        PriorityQueue&lt;Map.Entry&lt;Integer, Integer&gt;&gt; minHeap = 
            new PriorityQueue&lt;&gt;((a, b) -&gt; a.getValue() - b.getValue());
        for (Map.Entry&lt;Integer, Integer&gt; entry : freqMap.entrySet()) {
            minHeap.offer(entry);
            if (minHeap.size() &gt; k) {
                minHeap.poll();
            }
        }
        int[] result = new int[k];
        for (int i = k - 1; i &gt;= 0; i--) {
            result[i] = minHeap.poll().getKey();
        }
        return result;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;vector&gt;
#include &lt;unordered_map&gt;
#include &lt;queue&gt;
class Solution {
public:
    vector&lt;int&gt; topKFrequent(vector&lt;int&gt;&amp; nums, int k) {
        unordered_map&lt;int, int&gt; freqMap;
        for (int num : nums) {
            freqMap[num]++;
        }
        priority_queue&lt;pair&lt;int, int&gt;, vector&lt;pair&lt;int, int&gt;&gt;, 
                       greater&lt;pair&lt;int, int&gt;&gt;&gt; minHeap;
        for (auto&amp; pair : freqMap) {
            minHeap.push({pair.second, pair.first});
            if (minHeap.size() &gt; k) {
                minHeap.pop();
            }
        }
        vector&lt;int&gt; result;
        while (!minHeap.empty()) {
            result.push_back(minHeap.top().second);
            minHeap.pop();
        }
        return result;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n log k)  
**Space Complexity:** O(n)
## Approach 3: Bucket Sort (Optimal)
### Explanation
Use bucket sort where each bucket index represents a frequency. Since the maximum frequency cannot exceed the array length, we can create buckets for each possible frequency.
### Pseudocode
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">
freqMap = new HashMap()
for each num in nums:
    freqMap[num]++

buckets = array of lists, size = nums.length + 1
for each entry in freqMap:
    buckets[entry.frequency].add(entry.key)

result = []
for i = buckets.length - 1 down to 0:
    if buckets[i] is not empty:
        add all elements from buckets[i] to result
        if result.size() &gt;= k:
            break

return first k elements from result
</code></pre>
### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">import java.util.*;
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map&lt;Integer, Integer&gt; freqMap = new HashMap&lt;&gt;();
        for (int num : nums) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }
        List&lt;Integer&gt;[] buckets = new List[nums.length + 1];
        for (int i = 0; i &lt; buckets.length; i++) {
            buckets[i] = new ArrayList&lt;&gt;();
        }
        for (Map.Entry&lt;Integer, Integer&gt; entry : freqMap.entrySet()) {
            buckets[entry.getValue()].add(entry.getKey());
        }
        List&lt;Integer&gt; result = new ArrayList&lt;&gt;();
        for (int i = buckets.length - 1; i &gt;= 0 &amp;&amp; result.size() &lt; k; i--) {
            result.addAll(buckets[i]);
        }
        return result.subList(0, k).stream().mapToInt(i -&gt; i).toArray();
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;vector&gt;
#include &lt;unordered_map&gt;
class Solution {
public:
    vector&lt;int&gt; topKFrequent(vector&lt;int&gt;&amp; nums, int k) {
        unordered_map&lt;int, int&gt; freqMap;
        for (int num : nums) {
            freqMap[num]++;
        }
        vector&lt;vector&lt;int&gt;&gt; buckets(nums.size() + 1);
        for (auto&amp; pair : freqMap) {
            buckets[pair.second].push_back(pair.first);
        }
        vector&lt;int&gt; result;
        for (int i = buckets.size() - 1; i &gt;= 0 &amp;&amp; result.size() &lt; k; i--) {
            for (int num : buckets[i]) {
                result.push_back(num);
                if (result.size() == k) {
                    return result;
                }
            }
        }
        return result;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n)  
**Space Complexity:** O(n)
## Comparison Table
| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force (Sort by Frequency) | O(n log n) | O(n) | Use when k is close to n or when simplicity is preferred |
| Min Heap | O(n log k) | O(n) | Good when k is much smaller than n. Better than full sort when k << n |
| Bucket Sort (Optimal) | O(n) | O(n) | Most efficient. Best when you need optimal performance. Use when frequency distribution is important |