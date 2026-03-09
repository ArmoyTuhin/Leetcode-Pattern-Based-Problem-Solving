# Detect Squares

**Difficulty:** Medium

## Problem Statement

You are given a stream of points on the X-Y plane. Design an algorithm that:
- Adds new points from the stream into a data structure. Duplicate points are allowed and should be treated as different points.
- Given a query point, counts the number of ways to choose three points from the data structure such that the three points and the query point form an axis-aligned square with positive area.

An axis-aligned square is a square whose edges are all the same length and are either parallel or perpendicular to the x-axis and y-axis.

Implement the `DetectSquares` class:
- `DetectSquares()` Initializes the object with an empty data structure.
- `void add(int[] point)` Adds a new point `point = [x, y]` to the data structure.
- `int count(int[] point)` Counts the number of ways to form axis-aligned squares with point `point = [x, y]`.

**Example 1:**
```
Input
["DetectSquares", "add", "add", "add", "count", "count", "add", "count"]
[[], [[3, 10]], [[11, 2]], [[3, 2]], [[11, 10]], [[14, 8]], [[11, 2]], [[11, 10]]]
Output
[null, null, null, null, 1, 0, null, 2]

Explanation
DetectSquares detectSquares = new DetectSquares();
detectSquares.add([3, 10]);
detectSquares.add([11, 2]);
detectSquares.add([3, 2]);
detectSquares.count([11, 10]); // return 1. You can choose:
                               //   - The first, second, and third points
detectSquares.count([14, 8]);  // return 0. The query point cannot form a square with any points in the data structure.
detectSquares.add([11, 2]);    // Adding duplicate points is allowed.
detectSquares.count([11, 10]); // return 2. You can choose:
                               //   - The first, second, and third points
                               //   - The first, third, and fourth points
```

## Brief Explanation

For a query point (x, y), we need to find three other points that form a square. For each point (x1, y1) in the data structure, check if we can form a square with (x, y), (x1, y1), and two other points.

## Approach 1: Brute Force

### Explanation
For each query, check all combinations of three points to see if they form a square with the query point.

### Pseudocode
```
class DetectSquares:
    points = []
    
    add(point):
        points.add(point)
    
    count(query):
        count = 0
        for each combination of 3 points from points:
            if formsSquare(query, p1, p2, p3):
                count++
        return count
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class DetectSquares {
    private List<int[]> points;
    
    public DetectSquares() {
        points = new ArrayList<>();
    }
    
    public void add(int[] point) {
        points.add(point);
    }
    
    public int count(int[] point) {
        int count = 0;
        int n = points.size();
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (formsSquare(point, points.get(i), points.get(j), points.get(k))) {
                        count++;
                    }
                }
            }
        }
        
        return count;
    }
    
    private boolean formsSquare(int[] p1, int[] p2, int[] p3, int[] p4) {
        // Complex logic to check if four points form a square
        // This is inefficient
        return false;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <map>

class DetectSquares {
private:
    vector<pair<int, int>> points;
    
public:
    DetectSquares() {
    }
    
    void add(vector<int> point) {
        points.push_back({point[0], point[1]});
    }
    
    int count(vector<int> point) {
        int count = 0;
        int n = points.size();
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (formsSquare(point, points[i], points[j], points[k])) {
                        count++;
                    }
                }
            }
        }
        
        return count;
    }
    
private:
    bool formsSquare(vector<int> p1, pair<int, int> p2, pair<int, int> p3, pair<int, int> p4) {
        // Complex logic to check if four points form a square
        // This is inefficient
        return false;
    }
};
```

</div>

**Time Complexity:** O(n³) for count operation  
**Space Complexity:** O(n)

## Approach 2: Hash Map with Diagonal Points (Optimal)

### Explanation
Store points in a hash map. For query point (x, y), iterate through all points (x1, y1). If they form a diagonal, calculate the other two corner points and check if they exist.

### Pseudocode
```
class DetectSquares:
    pointCount = HashMap()
    
    add(point):
        pointCount[point]++
    
    count(query):
        x, y = query
        count = 0
        
        for each (x1, y1) in pointCount:
            if x != x1 and y != y1 and abs(x - x1) == abs(y - y1):
                // Diagonal point found
                p1 = (x, y1)
                p2 = (x1, y)
                count += pointCount[p1] * pointCount[p2] * pointCount[(x1, y1)]
        return count
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class DetectSquares {
    private Map<String, Integer> pointCount;
    
    public DetectSquares() {
        pointCount = new HashMap<>();
    }
    
    public void add(int[] point) {
        String key = point[0] + "," + point[1];
        pointCount.put(key, pointCount.getOrDefault(key, 0) + 1);
    }
    
    public int count(int[] point) {
        int x = point[0];
        int y = point[1];
        int count = 0;
        
        for (Map.Entry<String, Integer> entry : pointCount.entrySet()) {
            String[] parts = entry.getKey().split(",");
            int x1 = Integer.parseInt(parts[0]);
            int y1 = Integer.parseInt(parts[1]);
            
            if (x != x1 && y != y1 && Math.abs(x - x1) == Math.abs(y - y1)) {
                String p1 = x + "," + y1;
                String p2 = x1 + "," + y;
                count += pointCount.getOrDefault(p1, 0) * 
                         pointCount.getOrDefault(p2, 0) * 
                         entry.getValue();
            }
        }
        
        return count;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <unordered_map>
#include <string>
#include <sstream>

class DetectSquares {
private:
    unordered_map<string, int> pointCount;
    
    string getKey(int x, int y) {
        return to_string(x) + "," + to_string(y);
    }
    
public:
    DetectSquares() {
    }
    
    void add(vector<int> point) {
        string key = getKey(point[0], point[1]);
        pointCount[key]++;
    }
    
    int count(vector<int> point) {
        int x = point[0];
        int y = point[1];
        int result = 0;
        
        for (auto& entry : pointCount) {
            string key = entry.first;
            size_t pos = key.find(',');
            int x1 = stoi(key.substr(0, pos));
            int y1 = stoi(key.substr(pos + 1));
            
            if (x != x1 && y != y1 && abs(x - x1) == abs(y - y1)) {
                string p1 = getKey(x, y1);
                string p2 = getKey(x1, y);
                result += pointCount[p1] * pointCount[p2] * entry.second;
            }
        }
        
        return result;
    }
};
```

</div>

**Time Complexity:** O(n) for count operation  
**Space Complexity:** O(n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n³) for count | O(n) | Use only when number of points is very small |
| Hash Map with Diagonal Points (Optimal) | O(n) for count | O(n) | Most efficient. Uses hash map for fast lookups. Optimal solution |

