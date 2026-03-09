# Longest Repeating Character Replacement

**Difficulty:** Medium

## Problem Statement

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English letter. You can perform this operation at most `k` times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

**Example 1:**
```
Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.
```

**Example 2:**
```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
```

## Brief Explanation

We need to find the longest substring where we can replace at most k characters to make all characters the same. Use sliding window and track the maximum frequency character in the window.

## Approach 1: Brute Force

### Explanation
Check all possible substrings and count the maximum frequency character, then check if replacements needed <= k.

### Pseudocode
```
maxLength = 0
for i = 0 to n-1:
    for j = i to n-1:
        freq = count frequency of each char in substring
        maxFreq = max(freq)
        if (j - i + 1) - maxFreq <= k:
            maxLength = max(maxLength, j - i + 1)
return maxLength
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int characterReplacement(String s, int k) {
        int maxLength = 0;
        int n = s.length();
        
        for (int i = 0; i < n; i++) {
            int[] freq = new int[26];
            int maxFreq = 0;
            
            for (int j = i; j < n; j++) {
                freq[s.charAt(j) - 'A']++;
                maxFreq = Math.max(maxFreq, freq[s.charAt(j) - 'A']);
                
                int length = j - i + 1;
                if (length - maxFreq <= k) {
                    maxLength = Math.max(maxLength, length);
                }
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
#include <vector>
#include <algorithm>

class Solution {
public:
    int characterReplacement(string s, int k) {
        int maxLength = 0;
        int n = s.length();
        
        for (int i = 0; i < n; i++) {
            vector<int> freq(26, 0);
            int maxFreq = 0;
            
            for (int j = i; j < n; j++) {
                freq[s[j] - 'A']++;
                maxFreq = max(maxFreq, freq[s[j] - 'A']);
                
                int length = j - i + 1;
                if (length - maxFreq <= k) {
                    maxLength = max(maxLength, length);
                }
            }
        }
        
        return maxLength;
    }
};
```

</div>

**Time Complexity:** O(n²)  
**Space Complexity:** O(1)

## Approach 2: Sliding Window (Optimal)

### Explanation
Use a sliding window with two pointers. Track character frequencies. Expand window when valid (windowSize - maxFreq <= k), otherwise shrink from left.

### Pseudocode
```
freq = array of size 26
left = 0
maxFreq = 0
maxLength = 0

for right = 0 to n-1:
    freq[s[right]]++
    maxFreq = max(maxFreq, freq[s[right]])
    
    if (right - left + 1) - maxFreq > k:
        freq[s[left]]--
        left++
    
    maxLength = max(maxLength, right - left + 1)
return maxLength
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int[] freq = new int[26];
        int left = 0;
        int maxFreq = 0;
        int maxLength = 0;
        
        for (int right = 0; right < s.length(); right++) {
            freq[s.charAt(right) - 'A']++;
            maxFreq = Math.max(maxFreq, freq[s.charAt(right) - 'A']);
            
            if ((right - left + 1) - maxFreq > k) {
                freq[s.charAt(left) - 'A']--;
                left++;
            }
            
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
#include <vector>
#include <algorithm>

class Solution {
public:
    int characterReplacement(string s, int k) {
        vector<int> freq(26, 0);
        int left = 0;
        int maxFreq = 0;
        int maxLength = 0;
        
        for (int right = 0; right < s.length(); right++) {
            freq[s[right] - 'A']++;
            maxFreq = max(maxFreq, freq[s[right] - 'A']);
            
            if ((right - left + 1) - maxFreq > k) {
                freq[s[left] - 'A']--;
                left++;
            }
            
            maxLength = max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n²) | O(1) | Use only for very small strings |
| Sliding Window (Optimal) | O(n) | O(1) | Most efficient. Single pass through string. Optimal solution |

