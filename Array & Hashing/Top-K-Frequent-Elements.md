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
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freqMap = new HashMap<>();
        for (int num : nums) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }
        
        List<Integer> elements = new ArrayList<>(freqMap.keySet());
        elements.sort((a, b) -> freqMap.get(b) - freqMap.get(a));
        
        int[] result = new int[k];
        for (int i = 0; i < k; i++) {
            result[i] = elements.get(i);
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
#include <unordered_map>
#include <algorithm>

class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> freqMap;
        for (int num : nums) {
            freqMap[num]++;
        }
        
        vector<int> elements;
        for (auto& pair : freqMap) {
            elements.push_back(pair.first);
        }
        
        sort(elements.begin(), elements.end(), 
             [&freqMap](int a, int b) {
                 return freqMap[a] > freqMap[b];
             });
        
        return vector<int>(elements.begin(), elements.begin() + k);
    }
};

```

</div>

**Time Complexity:** O(n log n)  
**Space Complexity:** O(n)

## Approach 2: Min Heap (Priority Queue)

### Explanation
Use a min heap of size k to maintain the k most frequent elements. For each element, if the heap size is less than k, add it. Otherwise, if the current element's frequency is greater than the minimum in the heap, replace it.

### Pseudocode
```
freqMap = new HashMap()
for each num in nums:
    freqMap[num]++

minHeap = new PriorityQueue of size k (by frequency)
for each entry in freqMap:
    if minHeap.size() < k:
        minHeap.add(entry)
    else if entry.frequency > minHeap.peek().frequency:
        minHeap.poll()
        minHeap.add(entry)

return all elements from minHeap
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freqMap = new HashMap<>();
        for (int num : nums) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }
        
        PriorityQueue<Map.Entry<Integer, Integer>> minHeap = 
            new PriorityQueue<>((a, b) -> a.getValue() - b.getValue());
        
        for (Map.Entry<Integer, Integer> entry : freqMap.entrySet()) {
            minHeap.offer(entry);
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
        
        int[] result = new int[k];
        for (int i = k - 1; i >= 0; i--) {
            result[i] = minHeap.poll().getKey();
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
#include <unordered_map>
#include <queue>

class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> freqMap;
        for (int num : nums) {
            freqMap[num]++;
        }
        
        priority_queue<pair<int, int>, vector<pair<int, int>>, 
                       greater<pair<int, int>>> minHeap;
        
        for (auto& pair : freqMap) {
            minHeap.push({pair.second, pair.first});
            if (minHeap.size() > k) {
                minHeap.pop();
            }
        }
        
        vector<int> result;
        while (!minHeap.empty()) {
            result.push_back(minHeap.top().second);
            minHeap.pop();
        }
        return result;
    }
};

```

</div>

**Time Complexity:** O(n log k)  
**Space Complexity:** O(n)

## Approach 3: Bucket Sort (Optimal)

### Explanation
Use bucket sort where each bucket index represents a frequency. Since the maximum frequency cannot exceed the array length, we can create buckets for each possible frequency.

### Pseudocode
```
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
        if result.size() >= k:
            break

return first k elements from result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freqMap = new HashMap<>();
        for (int num : nums) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }
        
        List<Integer>[] buckets = new List[nums.length + 1];
        for (int i = 0; i < buckets.length; i++) {
            buckets[i] = new ArrayList<>();
        }
        
        for (Map.Entry<Integer, Integer> entry : freqMap.entrySet()) {
            buckets[entry.getValue()].add(entry.getKey());
        }
        
        List<Integer> result = new ArrayList<>();
        for (int i = buckets.length - 1; i >= 0 && result.size() < k; i--) {
            result.addAll(buckets[i]);
        }
        
        return result.subList(0, k).stream().mapToInt(i -> i).toArray();
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
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> freqMap;
        for (int num : nums) {
            freqMap[num]++;
        }
        
        vector<vector<int>> buckets(nums.size() + 1);
        for (auto& pair : freqMap) {
            buckets[pair.second].push_back(pair.first);
        }
        
        vector<int> result;
        for (int i = buckets.size() - 1; i >= 0 && result.size() < k; i--) {
            for (int num : buckets[i]) {
                result.push_back(num);
                if (result.size() == k) {
                    return result;
                }
            }
        }
        
        return result;
    }
};

```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force (Sort by Frequency) | O(n log n) | O(n) | Use when k is close to n or when simplicity is preferred |
| Min Heap | O(n log k) | O(n) | Good when k is much smaller than n. Better than full sort when k << n |
| Bucket Sort (Optimal) | O(n) | O(n) | Most efficient. Best when you need optimal performance. Use when frequency distribution is important |

