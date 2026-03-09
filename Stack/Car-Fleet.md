# Car Fleet

**Difficulty:** Medium

## Problem Statement

There are `n` cars going to the same destination along a one-lane road. The destination is `target` miles away.

You are given two integer arrays `position` and `speed`, both of length `n`, where `position[i]` is the position of the `i-th` car and `speed[i]` is the speed of the `i-th` car (in miles per hour).

A car can never pass another car ahead of it, but it can catch up to it and drive bumper to bumper at the same speed. The faster car will slow down to match the slower car's speed. The distance between these two cars is ignored (i.e., they are assumed to have the same position).

A car fleet is some non-empty set of cars driving at the same position and same speed. Note that a single car is also a car fleet.

If a car catches up to a car fleet right at the destination point, it will still be considered as one car fleet.

Return the number of car fleets that will arrive at the destination.

**Example 1:**
```
Input: target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]
Output: 3
Explanation:
The cars starting at 10 (speed 2) and 8 (speed 4) become a fleet, meeting each other at 12.
The car starting at 0 does not catch up to any other car, so it is a fleet by itself.
The cars starting at 5 (speed 1) and 3 (speed 3) become a fleet, meeting each other at 6.
```

**Example 2:**
```
Input: target = 10, position = [3], speed = [3]
Output: 1
```

**Example 3:**
```
Input: target = 100, position = [0,2,4], speed = [4,2,1]
Output: 1
```

## Brief Explanation

Cars form fleets when faster cars catch up to slower cars. We need to calculate when each car reaches the destination and determine how many distinct fleets arrive.

## Approach 1: Brute Force

### Explanation
For each car, calculate its arrival time. Then check which cars will form fleets by comparing arrival times and positions.

### Pseudocode
```
n = length(position)
times = new array of size n

for i = 0 to n-1:
    times[i] = (target - position[i]) / speed[i]

// Sort by position and check fleets
// Complex logic to determine fleets
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int carFleet(int target, int[] position, int[] speed) {
        int n = position.length;
        double[] times = new double[n];
        
        for (int i = 0; i < n; i++) {
            times[i] = (double)(target - position[i]) / speed[i];
        }
        
        // Create pairs and sort by position
        int[][] cars = new int[n][2];
        for (int i = 0; i < n; i++) {
            cars[i][0] = position[i];
            cars[i][1] = i;
        }
        Arrays.sort(cars, (a, b) -> b[0] - a[0]);
        
        int fleets = 0;
        double maxTime = 0;
        
        for (int[] car : cars) {
            int idx = car[1];
            if (times[idx] > maxTime) {
                fleets++;
                maxTime = times[idx];
            }
        }
        
        return fleets;
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <algorithm>
#include <map>

class Solution {
public:
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        int n = position.size();
        vector<pair<int, double>> cars;
        
        for (int i = 0; i < n; i++) {
            double time = (double)(target - position[i]) / speed[i];
            cars.push_back({position[i], time});
        }
        
        sort(cars.begin(), cars.end(), greater<pair<int, double>>());
        
        int fleets = 0;
        double maxTime = 0;
        
        for (auto& car : cars) {
            if (car.second > maxTime) {
                fleets++;
                maxTime = car.second;
            }
        }
        
        return fleets;
    }
};
```

</div>

**Time Complexity:** O(n log n)  
**Space Complexity:** O(n)

## Approach 2: Stack (Optimal)

### Explanation
Sort cars by position (descending). Use a stack to track fleets. A car forms a new fleet if it arrives later than the car in front of it.

### Pseudocode
```
n = length(position)
cars = create pairs (position, time to target)
sort cars by position descending

stack = new Stack()
for each car in cars:
    if stack is empty or car.time > stack.top():
        stack.push(car.time)
return stack.size()
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public int carFleet(int target, int[] position, int[] speed) {
        int n = position.length;
        double[][] cars = new double[n][2];
        
        for (int i = 0; i < n; i++) {
            cars[i][0] = position[i];
            cars[i][1] = (double)(target - position[i]) / speed[i];
        }
        
        Arrays.sort(cars, (a, b) -> Double.compare(b[0], a[0]));
        
        Stack<Double> stack = new Stack<>();
        for (double[] car : cars) {
            if (stack.isEmpty() || car[1] > stack.peek()) {
                stack.push(car[1]);
            }
        }
        
        return stack.size();
    }
}
```

</div>

### C++ Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```cpp
#include <vector>
#include <algorithm>
#include <stack>

class Solution {
public:
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        int n = position.size();
        vector<pair<int, double>> cars;
        
        for (int i = 0; i < n; i++) {
            double time = (double)(target - position[i]) / speed[i];
            cars.push_back({position[i], time});
        }
        
        sort(cars.begin(), cars.end(), greater<pair<int, double>>());
        
        stack<double> st;
        for (auto& car : cars) {
            if (st.empty() || car.second > st.top()) {
                st.push(car.second);
            }
        }
        
        return st.size();
    }
};
```

</div>

**Time Complexity:** O(n log n)  
**Space Complexity:** O(n)

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force | O(n log n) | O(n) | More complex logic, harder to understand |
| Stack (Optimal) | O(n log n) | O(n) | Cleaner logic. Stack naturally tracks fleets. Optimal solution |

