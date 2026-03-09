# Longest Substring Without Repeating Characters

**Difficulty:** Medium

## Problem Statement

Given a string `s`, find the length of the longest substring without repeating characters.

**Example 1:**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Brief Explanation

We need to find the longest substring with all unique characters. Use a sliding window with a hash map to track character frequencies and adjust the window when duplicates are found.

## Approach 1: Brute Force

### Explanation
Check all possible substrings and find the longest one without repeating characters.

### Pseudocode
```
maxLength = 0
for i = 0 to n-1:
    seen = new Set()
    for j = i to n-1:
        if s[j] in seen:
            break
        seen.add(s[j])
        maxLength = max(maxLength, j - i + 1)
return maxLength
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        int maxLength = 0;
        int n = s.length();
        
        for (int i = 0; i < n; i++) {
            Set<Character> seen = new HashSet<>();
            for (int j = i; j < n; j++) {
                if (seen.contains(s.charAt(j))) {
                    break;
                }
                seen.add(s.charAt(j));
                maxLength = Math.max(maxLength, j - i + 1);
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
#include <string>
#include <unordered_set>
#include <algorithm>

class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int maxLength = 0;
        int n = s.length();
        
        for (int i = 0; i < n; i++) {
            unordered_set<char> seen;
            for (int j = i; j < n; j++) {
                if (seen.find(s[j]) != seen.end()) {
                    break;
                }
                seen.insert(s[j]);
                maxLength = max(maxLength, j - i + 1);
            }
        }
        
        return maxLength;
    }
};
```

</div>

**Time Complexity:** O(n²)  
**Space Complexity:** O(min(n, m)) where m is character set size

## Approach 2: Sliding Window with Hash Map (Optimal)

### Explanation
Use a sliding window with two pointers. Use a hash map to track the last index of each character. When a duplicate is found, move the left pointer to the right of the last occurrence.

### Pseudocode
```
map = new HashMap()
left = 0
maxLength = 0

for right = 0 to n-1:
    if s[right] in map:
        left = max(left, map[s[right]] + 1)
    map[s[right]] = right
    maxLength = max(maxLength, right - left + 1)
return maxLength
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> map = new HashMap<>();
        int left = 0;
        int maxLength = 0;
        
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            if (map.containsKey(c)) {
                left = Math.max(left, map.get(c) + 1);
            }
            map.put(c, right);
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <string>
#include <unordered_map>
#include <algorithm>

class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> map;
        int left = 0;
        int maxLength = 0;
        
        for (int right = 0; right < s.length(); right++) {
            if (map.find(s[right]) != map.end()) {
                left = max(left, map[s[right]] + 1);
            }
            map[s[right]] = right;
            maxLength = max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(min(n, m)) where m is character set size

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n²) | O(min(n, m)) | Use only for very small strings |
| Sliding Window with Hash Map (Optimal) | O(n) | O(min(n, m)) | Most efficient. Single pass through string. Optimal solution |

