# Valid Anagram

**Difficulty:** Easy  

## Problem Statement

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**
```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**
```
Input: s = "rat", t = "car"
Output: false
```

## Brief Explanation

We need to check if two strings contain the same characters with the same frequencies. The challenge is to do this efficiently and handle edge cases like different string lengths.

## Approach 1: Brute Force (Character Count Comparison)

### Explanation
Count the frequency of each character in both strings and compare the counts. If all character counts match, the strings are anagrams.

### Pseudocode
```
if length(s) != length(t):
    return false

countS = array of size 26
countT = array of size 26

for i = 0 to length(s)-1:
    countS[s[i] - 'a']++
    countT[t[i] - 'a']++

for i = 0 to 25:
    if countS[i] != countT[i]:
        return false
return true
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int[] countS = new int[26];
        int[] countT = new int[26];
        for (int i = 0; i &lt; s.length(); i++) {
            countS[s.charAt(i) - 'a']++;
            countT[t.charAt(i) - 'a']++;
        }
        for (int i = 0; i &lt; 26; i++) {
            if (countS[i] != countT[i]) {
                return false;
            }
        }
        return true;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;string&gt;
#include &lt;vector&gt;
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.length() != t.length()) {
            return false;
        }
        vector&lt;int&gt; countS(26, 0);
        vector&lt;int&gt; countT(26, 0);
        for (int i = 0; i &lt; s.length(); i++) {
            countS[s[i] - 'a']++;
            countT[t[i] - 'a']++;
        }
        for (int i = 0; i &lt; 26; i++) {
            if (countS[i] != countT[i]) {
                return false;
            }
        }
        return true;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n)  
**Space Complexity:** O(1) - fixed size array of 26
## Approach 2: Sorting
### Explanation
Sort both strings and compare them. If they are equal after sorting, they are anagrams.
### Pseudocode
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">
if length(s) != length(t):
    return false

sort(s)
sort(t)
return s == t
</code></pre>
### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">import java.util.Arrays;
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        char[] sArray = s.toCharArray();
        char[] tArray = t.toCharArray();
        Arrays.sort(sArray);
        Arrays.sort(tArray);
        return Arrays.equals(sArray, tArray);
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;string&gt;
#include &lt;algorithm&gt;
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.length() != t.length()) {
            return false;
        }
        sort(s.begin(), s.end());
        sort(t.begin(), t.end());
        return s == t;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n log n)  
**Space Complexity:** O(1) for C++, O(n) for Java (due to toCharArray)
## Approach 3: Single Array Count (Optimal)
### Explanation
Use a single array to count characters. Increment for characters in `s` and decrement for characters in `t`. If all counts are zero, they are anagrams.
### Pseudocode
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">
if length(s) != length(t):
    return false

count = array of size 26

for i = 0 to length(s)-1:
    count[s[i] - 'a']++
    count[t[i] - 'a']--

for i = 0 to 25:
    if count[i] != 0:
        return false
return true
</code></pre>
### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int[] count = new int[26];
        for (int i = 0; i &lt; s.length(); i++) {
            count[s.charAt(i) - 'a']++;
            count[t.charAt(i) - 'a']--;
        }
        for (int i = 0; i &lt; 26; i++) {
            if (count[i] != 0) {
                return false;
            }
        }
        return true;
    }
}</code></pre>
</div>
</div>
### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto; margin: 10px 0;">
<pre style="background-color: #1e1e1e; color: #d4d4d4; margin: 0; padding: 0; font-family: 'Consolas', 'Monaco', 'Courier New', monospace; font-size: 14px; line-height: 1.5;"><code style="background-color: #1e1e1e; color: #d4d4d4;">#include &lt;string&gt;
#include &lt;vector&gt;
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.length() != t.length()) {
            return false;
        }
        vector&lt;int&gt; count(26, 0);
        for (int i = 0; i &lt; s.length(); i++) {
            count[s[i] - 'a']++;
            count[t[i] - 'a']--;
        }
        for (int i = 0; i &lt; 26; i++) {
            if (count[i] != 0) {
                return false;
            }
        }
        return true;
    }
};</code></pre>
</div>
</div>
**Time Complexity:** O(n)  
**Space Complexity:** O(1) - fixed size array of 26
## Comparison Table
| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force (Two Arrays) | O(n) | O(1) | Good for understanding but slightly less efficient |
| Sorting | O(n log n) | O(1) for C++, O(n) for Java | Use when you need sorted strings anyway |
| Single Array Count (Optimal) | O(n) | O(1) | Most efficient. Best approach for this problem |