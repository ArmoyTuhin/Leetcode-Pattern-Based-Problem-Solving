# Valid Palindrome

**Difficulty:** Easy

## Problem Statement

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward.

Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

**Example 1:**
```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2:**
```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Example 3:**
```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

## Brief Explanation

We need to check if a string is a palindrome after removing non-alphanumeric characters and converting to lowercase. The challenge is to do this efficiently using two pointers.

## Approach 1: Brute Force (Create Clean String)

### Explanation
Create a new string with only alphanumeric characters in lowercase, then check if it's a palindrome by comparing with its reverse.

### Pseudocode
```
cleanStr = ""
for each char in s:
    if char is alphanumeric:
        cleanStr += toLowerCase(char)

return cleanStr == reverse(cleanStr)
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public boolean isPalindrome(String s) {
        StringBuilder clean = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (Character.isLetterOrDigit(c)) {
                clean.append(Character.toLowerCase(c));
            }
        }
        return clean.toString().equals(clean.reverse().toString());
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <string>
#include <algorithm>
#include <cctype>

class Solution {
public:
    bool isPalindrome(string s) {
        string clean = "";
        for (char c : s) {
            if (isalnum(c)) {
                clean += tolower(c);
            }
        }
        string reversed = clean;
        reverse(reversed.begin(), reversed.end());
        return clean == reversed;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

## Approach 2: Two Pointers (Optimal)

### Explanation
Use two pointers, one from the start and one from the end. Skip non-alphanumeric characters and compare characters at both pointers.

### Pseudocode
```
left = 0
right = length(s) - 1

while left < right:
    while left < right and not isAlphanumeric(s[left]):
        left++
    while left < right and not isAlphanumeric(s[right]):
        right--
    
    if toLowerCase(s[left]) != toLowerCase(s[right]):
        return false
    
    left++
    right--

return true
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        
        while (left < right) {
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }
            
            if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                return false;
            }
            
            left++;
            right--;
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
#include <cctype>

class Solution {
public:
    bool isPalindrome(string s) {
        int left = 0;
        int right = s.length() - 1;
        
        while (left < right) {
            while (left < right && !isalnum(s[left])) {
                left++;
            }
            while (left < right && !isalnum(s[right])) {
                right--;
            }
            
            if (tolower(s[left]) != tolower(s[right])) {
                return false;
            }
            
            left++;
            right--;
        }
        
        return true;
    }
};
```

</div>

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force (Create Clean String) | O(n) | O(n) | Simple to understand but uses extra space |
| Two Pointers (Optimal) | O(n) | O(1) | Most efficient. Uses constant space. Best for this problem |

