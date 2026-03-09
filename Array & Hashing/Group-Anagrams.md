# Group Anagrams

**Difficulty:** Medium  

## Problem Statement

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**
```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**Example 2:**
```
Input: strs = [""]
Output: [[""]]
```

**Example 3:**
```
Input: strs = ["a"]
Output: [["a"]]
```

## Brief Explanation

We need to group strings that are anagrams of each other. The challenge is to efficiently identify which strings are anagrams and group them together.

## Approach 1: Brute Force (Character Count as Key)

### Explanation
For each string, create a character count array and use it as a key. Compare each string with all others to group anagrams. This approach uses character frequency arrays as keys.

### Pseudocode
```
result = []
used = boolean array of size n

for i = 0 to n-1:
    if used[i]:
        continue
    group = [strs[i]]
    countI = getCharacterCount(strs[i])
    
    for j = i+1 to n-1:
        if used[j]:
            continue
        countJ = getCharacterCount(strs[j])
        if countI == countJ:
            group.add(strs[j])
            used[j] = true
    
    result.add(group)
    used[i] = true

return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> result = new ArrayList<>();
        boolean[] used = new boolean[strs.length];
        
        for (int i = 0; i < strs.length; i++) {
            if (used[i]) continue;
            
            List<String> group = new ArrayList<>();
            group.add(strs[i]);
            int[] countI = getCharacterCount(strs[i]);
            
            for (int j = i + 1; j < strs.length; j++) {
                if (used[j]) continue;
                
                int[] countJ = getCharacterCount(strs[j]);
                if (Arrays.equals(countI, countJ)) {
                    group.add(strs[j]);
                    used[j] = true;
                }
            }
            
            result.add(group);
            used[i] = true;
        }
        
        return result;
    }
    
    private int[] getCharacterCount(String s) {
        int[] count = new int[26];
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }
        return count;
    }
}

```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <string>
#include <algorithm>

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> result;
        vector<bool> used(strs.size(), false);
        
        for (int i = 0; i < strs.size(); i++) {
            if (used[i]) continue;
            
            vector<string> group;
            group.push_back(strs[i]);
            vector<int> countI = getCharacterCount(strs[i]);
            
            for (int j = i + 1; j < strs.size(); j++) {
                if (used[j]) continue;
                
                vector<int> countJ = getCharacterCount(strs[j]);
                if (countI == countJ) {
                    group.push_back(strs[j]);
                    used[j] = true;
                }
            }
            
            result.push_back(group);
            used[i] = true;
        }
        
        return result;
    }
    
private:
    vector<int> getCharacterCount(string s) {
        vector<int> count(26, 0);
        for (char c : s) {
            count[c - 'a']++;
        }
        return count;
    }
};

```

</div>

**Time Complexity:** O(n² * k) where k is average string length  
**Space Complexity:** O(n * k)

## Approach 2: Sorting as Key

### Explanation
Sort each string and use the sorted string as a key in a hash map. All anagrams will have the same sorted key.

### Pseudocode
```
map = new HashMap<String, List<String>>()

for each str in strs:
    sortedStr = sort(str)
    if sortedStr not in map:
        map[sortedStr] = new List()
    map[sortedStr].add(str)

return all values from map
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        
        for (String str : strs) {
            char[] chars = str.toCharArray();
            Arrays.sort(chars);
            String sortedStr = new String(chars);
            
            map.putIfAbsent(sortedStr, new ArrayList<>());
            map.get(sortedStr).add(str);
        }
        
        return new ArrayList<>(map.values());
    }
}

```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> map;
        
        for (string str : strs) {
            string sortedStr = str;
            sort(sortedStr.begin(), sortedStr.end());
            map[sortedStr].push_back(str);
        }
        
        vector<vector<string>> result;
        for (auto& pair : map) {
            result.push_back(pair.second);
        }
        
        return result;
    }
};

```

</div>

**Time Complexity:** O(n * k log k) where k is average string length  
**Space Complexity:** O(n * k)

## Approach 3: Character Count as Key (Optimal)

### Explanation
Use character frequency count as the key instead of sorting. This avoids the O(k log k) sorting cost and uses O(k) time to build the key.

### Pseudocode
```
map = new HashMap<String, List<String>>()

for each str in strs:
    key = getCharacterCountKey(str)
    if key not in map:
        map[key] = new List()
    map[key].add(str)

return all values from map

getCharacterCountKey(str):
    count = array of size 26
    for each char in str:
        count[char - 'a']++
    return count as string key
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        
        for (String str : strs) {
            String key = getCharacterCountKey(str);
            map.putIfAbsent(key, new ArrayList<>());
            map.get(key).add(str);
        }
        
        return new ArrayList<>(map.values());
    }
    
    private String getCharacterCountKey(String s) {
        int[] count = new int[26];
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }
        
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 26; i++) {
            sb.append('#');
            sb.append(count[i]);
        }
        return sb.toString();
    }
}

```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <string>
#include <unordered_map>

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> map;
        
        for (string str : strs) {
            string key = getCharacterCountKey(str);
            map[key].push_back(str);
        }
        
        vector<vector<string>> result;
        for (auto& pair : map) {
            result.push_back(pair.second);
        }
        
        return result;
    }
    
private:
    string getCharacterCountKey(string s) {
        vector<int> count(26, 0);
        for (char c : s) {
            count[c - 'a']++;
        }
        
        string key;
        for (int i = 0; i < 26; i++) {
            key += to_string(count[i]) + "#";
        }
        return key;
    }
};

```

</div>

**Time Complexity:** O(n * k) where k is average string length  
**Space Complexity:** O(n * k)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n² * k) | O(n * k) | Use only for very small inputs or educational purposes |
| Sorting as Key | O(n * k log k) | O(n * k) | Good balance between simplicity and efficiency. Easy to understand and implement |
| Character Count as Key (Optimal) | O(n * k) | O(n * k) | Most efficient. Avoids sorting overhead. Best for production code when performance matters |

