# Time Based Key Value Store

**Difficulty:** Medium

## Problem Statement

Design a time-based key-value data structure that can store multiple values for the same key at different timestamps and retrieve the key's value at a certain timestamp.

Implement the `TimeMap` class:
- `TimeMap()` Initializes the object of the data structure.
- `void set(String key, String value, int timestamp)` Stores the key `key` with the value `value` at the given time `timestamp`.
- `String get(String key, int timestamp)` Returns a value such that `set` was called previously, with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value associated with the largest `timestamp_prev`. If there are no values, it returns `""`.

**Example 1:**
```
Input
["TimeMap", "set", "get", "get", "set", "get", "get"]
[[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]
Output
[null, null, "bar", "bar", null, "bar2", "bar2"]

Explanation
TimeMap timeMap = new TimeMap();
timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.
timeMap.get("foo", 1);         // return "bar"
timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
timeMap.set("foo", "bar2", 4); // store the key "foo" and value "bar2" along with timestamp = 4.
timeMap.get("foo", 4);         // return "bar2"
timeMap.get("foo", 5);         // return "bar2"
```

## Brief Explanation

We need to store key-value pairs with timestamps and retrieve the value for a key at a given timestamp. Use a map of lists and binary search to find the appropriate value.

## Approach 1: Linear Search

### Explanation
For each key, store a list of (timestamp, value) pairs. When getting, search linearly through the list.

### Pseudocode
```
class TimeMap:
    map = {}
    
    set(key, value, timestamp):
        if key not in map:
            map[key] = []
        map[key].append((timestamp, value))
    
    get(key, timestamp):
        if key not in map:
            return ""
        values = map[key]
        for i = values.length-1 down to 0:
            if values[i].timestamp <= timestamp:
                return values[i].value
        return ""
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class TimeMap {
    private Map<String, List<Pair<Integer, String>>> map;
    
    public TimeMap() {
        map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        map.putIfAbsent(key, new ArrayList<>());
        map.get(key).add(new Pair<>(timestamp, value));
    }
    
    public String get(String key, int timestamp) {
        if (!map.containsKey(key)) {
            return "";
        }
        
        List<Pair<Integer, String>> values = map.get(key);
        for (int i = values.size() - 1; i >= 0; i--) {
            if (values.get(i).getKey() <= timestamp) {
                return values.get(i).getValue();
            }
        }
        return "";
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <unordered_map>
#include <vector>
#include <string>

class TimeMap {
private:
    unordered_map<string, vector<pair<int, string>>> map;
    
public:
    TimeMap() {
    }
    
    void set(string key, string value, int timestamp) {
        map[key].push_back({timestamp, value});
    }
    
    string get(string key, int timestamp) {
        if (map.find(key) == map.end()) {
            return "";
        }
        
        vector<pair<int, string>>& values = map[key];
        for (int i = values.size() - 1; i >= 0; i--) {
            if (values[i].first <= timestamp) {
                return values[i].second;
            }
        }
        return "";
    }
};
```

</div>

**Time Complexity:** O(n) for get operation  
**Space Complexity:** O(n)

## Approach 2: Binary Search (Optimal)

### Explanation
Store (timestamp, value) pairs in sorted order. Use binary search to find the largest timestamp <= target timestamp.

### Pseudocode
```
class TimeMap:
    map = {}
    
    set(key, value, timestamp):
        if key not in map:
            map[key] = []
        map[key].append((timestamp, value))
    
    get(key, timestamp):
        if key not in map:
            return ""
        values = map[key]
        left = 0
        right = values.length - 1
        result = ""
        
        while left <= right:
            mid = left + (right - left) / 2
            if values[mid].timestamp <= timestamp:
                result = values[mid].value
                left = mid + 1
            else:
                right = mid - 1
        return result
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class TimeMap {
    private Map<String, List<Pair<Integer, String>>> map;
    
    public TimeMap() {
        map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        map.putIfAbsent(key, new ArrayList<>());
        map.get(key).add(new Pair<>(timestamp, value));
    }
    
    public String get(String key, int timestamp) {
        if (!map.containsKey(key)) {
            return "";
        }
        
        List<Pair<Integer, String>> values = map.get(key);
        int left = 0;
        int right = values.size() - 1;
        String result = "";
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (values.get(mid).getKey() <= timestamp) {
                result = values.get(mid).getValue();
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return result;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <unordered_map>
#include <vector>
#include <string>

class TimeMap {
private:
    unordered_map<string, vector<pair<int, string>>> map;
    
public:
    TimeMap() {
    }
    
    void set(string key, string value, int timestamp) {
        map[key].push_back({timestamp, value});
    }
    
    string get(string key, int timestamp) {
        if (map.find(key) == map.end()) {
            return "";
        }
        
        vector<pair<int, string>>& values = map[key];
        int left = 0;
        int right = values.size() - 1;
        string result = "";
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (values[mid].first <= timestamp) {
                result = values[mid].second;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return result;
    }
};
```

</div>

**Time Complexity:** O(log n) for get operation  
**Space Complexity:** O(n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Linear Search | O(n) for get | O(n) | Use only when number of values per key is very small |
| Binary Search (Optimal) | O(log n) for get | O(n) | Most efficient. Optimal for get operations. Best solution |

