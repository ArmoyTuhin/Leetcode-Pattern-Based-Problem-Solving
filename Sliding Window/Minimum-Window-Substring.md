# Minimum Window Substring

**Difficulty:** Hard

## Problem Statement

Given two strings `s` and `t`, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such window, return the empty string `""`.

The testcases will be generated such that the answer is unique.

**Example 1:**
```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

**Example 2:**
```
Input: s = "a", t = "a"
Output: "a"
```

**Example 3:**
```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
```

## Brief Explanation

We need to find the smallest substring in s that contains all characters from t. Use a sliding window approach with two pointers and track character frequencies.

## Approach 1: Brute Force

### Explanation
Check all possible substrings of s and find the minimum one that contains all characters from t.

### Pseudocode
```
minWindow = ""
minLength = infinity

for i = 0 to n-1:
    for j = i to n-1:
        substring = s.substring(i, j+1)
        if containsAll(substring, t):
            if j - i + 1 < minLength:
                minLength = j - i + 1
                minWindow = substring
return minWindow
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public String minWindow(String s, String t) {
        String minWindow = "";
        int minLength = Integer.MAX_VALUE;
        int n = s.length();
        
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                String substring = s.substring(i, j + 1);
                if (containsAll(substring, t)) {
                    if (j - i + 1 < minLength) {
                        minLength = j - i + 1;
                        minWindow = substring;
                    }
                }
            }
        }
        
        return minWindow;
    }
    
    private boolean containsAll(String s, String t) {
        int[] freq = new int[128];
        for (char c : t.toCharArray()) {
            freq[c]++;
        }
        
        for (char c : s.toCharArray()) {
            freq[c]--;
        }
        
        for (int count : freq) {
            if (count > 0) {
                return false;
            }
        }
        return true;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <string>
#include <vector>
#include <climits>

class Solution {
public:
    string minWindow(string s, string t) {
        string minWindow = "";
        int minLength = INT_MAX;
        int n = s.length();
        
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                string substring = s.substr(i, j - i + 1);
                if (containsAll(substring, t)) {
                    if (j - i + 1 < minLength) {
                        minLength = j - i + 1;
                        minWindow = substring;
                    }
                }
            }
        }
        
        return minWindow;
    }
    
private:
    bool containsAll(string s, string t) {
        vector<int> freq(128, 0);
        for (char c : t) {
            freq[c]++;
        }
        
        for (char c : s) {
            freq[c]--;
        }
        
        for (int count : freq) {
            if (count > 0) {
                return false;
            }
        }
        return true;
    }
};
```

</div>

**Time Complexity:** O(n³)  
**Space Complexity:** O(1)

## Approach 2: Sliding Window (Optimal)

### Explanation
Use two pointers to maintain a sliding window. Expand right pointer until all characters from t are included, then shrink left pointer to find minimum window.

### Pseudocode
```
tFreq = count frequency of t
windowFreq = array of size 128
left = 0
required = number of unique characters in t
formed = 0
minLength = infinity
minLeft = 0

for right = 0 to n-1:
    windowFreq[s[right]]++
    if windowFreq[s[right]] == tFreq[s[right]]:
        formed++
    
    while formed == required:
        if right - left + 1 < minLength:
            minLength = right - left + 1
            minLeft = left
        
        windowFreq[s[left]]--
        if windowFreq[s[left]] < tFreq[s[left]]:
            formed--
        left++

return minLength == infinity ? "" : s.substring(minLeft, minLeft + minLength)
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] tFreq = new int[128];
        for (char c : t.toCharArray()) {
            tFreq[c]++;
        }
        
        int[] windowFreq = new int[128];
        int left = 0;
        int required = 0;
        for (int count : tFreq) {
            if (count > 0) required++;
        }
        int formed = 0;
        int minLength = Integer.MAX_VALUE;
        int minLeft = 0;
        
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            windowFreq[c]++;
            
            if (windowFreq[c] == tFreq[c]) {
                formed++;
            }
            
            while (formed == required) {
                if (right - left + 1 < minLength) {
                    minLength = right - left + 1;
                    minLeft = left;
                }
                
                char leftChar = s.charAt(left);
                windowFreq[leftChar]--;
                if (windowFreq[leftChar] < tFreq[leftChar]) {
                    formed--;
                }
                left++;
            }
        }
        
        return minLength == Integer.MAX_VALUE ? "" : s.substring(minLeft, minLeft + minLength);
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <string>
#include <vector>
#include <climits>

class Solution {
public:
    string minWindow(string s, string t) {
        vector<int> tFreq(128, 0);
        for (char c : t) {
            tFreq[c]++;
        }
        
        vector<int> windowFreq(128, 0);
        int left = 0;
        int required = 0;
        for (int count : tFreq) {
            if (count > 0) required++;
        }
        int formed = 0;
        int minLength = INT_MAX;
        int minLeft = 0;
        
        for (int right = 0; right < s.length(); right++) {
            windowFreq[s[right]]++;
            
            if (windowFreq[s[right]] == tFreq[s[right]]) {
                formed++;
            }
            
            while (formed == required) {
                if (right - left + 1 < minLength) {
                    minLength = right - left + 1;
                    minLeft = left;
                }
                
                windowFreq[s[left]]--;
                if (windowFreq[s[left]] < tFreq[s[left]]) {
                    formed--;
                }
                left++;
            }
        }
        
        return minLength == INT_MAX ? "" : s.substr(minLeft, minLength);
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n³) | O(1) | Use only for very small strings |
| Sliding Window (Optimal) | O(n) | O(1) | Most efficient. Single pass through string. Optimal solution |

