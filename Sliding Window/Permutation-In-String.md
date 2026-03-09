# Permutation In String

**Difficulty:** Medium

## Problem Statement

Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

**Example 1:**
```
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2:**
```
Input: s1 = "ab", s2 = "eidboaoo"
Output: false
```

## Brief Explanation

We need to check if any substring of s2 has the same character frequency as s1. Use a sliding window of size s1.length() and compare character frequencies.

## Approach 1: Brute Force

### Explanation
Check all substrings of s2 with length equal to s1.length() and compare character frequencies.

### Pseudocode
```
s1Freq = count frequency of s1
for i = 0 to s2.length() - s1.length():
    substring = s2.substring(i, i + s1.length())
    s2Freq = count frequency of substring
    if s1Freq == s2Freq:
        return true
return false
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int[] s1Freq = new int[26];
        for (char c : s1.toCharArray()) {
            s1Freq[c - 'a']++;
        }
        
        int n = s2.length();
        int m = s1.length();
        
        for (int i = 0; i <= n - m; i++) {
            String substring = s2.substring(i, i + m);
            int[] s2Freq = new int[26];
            for (char c : substring.toCharArray()) {
                s2Freq[c - 'a']++;
            }
            
            if (Arrays.equals(s1Freq, s2Freq)) {
                return true;
            }
        }
        
        return false;
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
    bool checkInclusion(string s1, string s2) {
        vector<int> s1Freq(26, 0);
        for (char c : s1) {
            s1Freq[c - 'a']++;
        }
        
        int n = s2.length();
        int m = s1.length();
        
        for (int i = 0; i <= n - m; i++) {
            vector<int> s2Freq(26, 0);
            for (int j = i; j < i + m; j++) {
                s2Freq[s2[j] - 'a']++;
            }
            
            if (s1Freq == s2Freq) {
                return true;
            }
        }
        
        return false;
    }
};
```

</div>

**Time Complexity:** O(n * m) where n = s2.length(), m = s1.length()  
**Space Complexity:** O(1)

## Approach 2: Sliding Window (Optimal)

### Explanation
Use a sliding window of fixed size s1.length(). Maintain character frequencies in the window and compare with s1 frequencies.

### Pseudocode
```
s1Freq = count frequency of s1
windowFreq = array of size 26
left = 0

for right = 0 to s2.length()-1:
    windowFreq[s2[right]]++
    
    if right - left + 1 == s1.length():
        if windowFreq == s1Freq:
            return true
        windowFreq[s2[left]]--
        left++
return false
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int[] s1Freq = new int[26];
        for (char c : s1.toCharArray()) {
            s1Freq[c - 'a']++;
        }
        
        int[] windowFreq = new int[26];
        int left = 0;
        
        for (int right = 0; right < s2.length(); right++) {
            windowFreq[s2.charAt(right) - 'a']++;
            
            if (right - left + 1 == s1.length()) {
                if (Arrays.equals(s1Freq, windowFreq)) {
                    return true;
                }
                windowFreq[s2.charAt(left) - 'a']--;
                left++;
            }
        }
        
        return false;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <string>
#include <vector>

class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        vector<int> s1Freq(26, 0);
        for (char c : s1) {
            s1Freq[c - 'a']++;
        }
        
        vector<int> windowFreq(26, 0);
        int left = 0;
        
        for (int right = 0; right < s2.length(); right++) {
            windowFreq[s2[right] - 'a']++;
            
            if (right - left + 1 == s1.length()) {
                if (s1Freq == windowFreq) {
                    return true;
                }
                windowFreq[s2[left] - 'a']--;
                left++;
            }
        }
        
        return false;
    }
};
```

</div>

**Time Complexity:** O(n) where n = s2.length()  
**Space Complexity:** O(1)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n * m) | O(1) | Use only when s1 is very small |
| Sliding Window (Optimal) | O(n) | O(1) | Most efficient. Single pass through s2. Optimal solution |

