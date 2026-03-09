# Valid Sudoku

**Difficulty:** Medium  

## Problem Statement

Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**
- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

**Example 1:**
```
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]

Output: true
```

**Example 2:**
```
Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]

Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

## Brief Explanation

We need to validate that a Sudoku board follows the three rules: no duplicates in rows, columns, or 3x3 sub-boxes. The challenge is to efficiently check all three constraints without redundant work.

## Approach 1: Brute Force (Three Separate Checks)

### Explanation
Check rows, columns, and sub-boxes separately. For each, use a set to detect duplicates.

### Pseudocode
```
function isValidSudoku(board):
    // Check rows
    for i = 0 to 8:
        seen = new Set()
        for j = 0 to 8:
            if board[i][j] != '.':
                if board[i][j] in seen:
                    return false
                seen.add(board[i][j])
    
    // Check columns
    for j = 0 to 8:
        seen = new Set()
        for i = 0 to 8:
            if board[i][j] != '.':
                if board[i][j] in seen:
                    return false
                seen.add(board[i][j])
    
    // Check sub-boxes
    for box = 0 to 8:
        seen = new Set()
        startRow = (box / 3) * 3
        startCol = (box % 3) * 3
        for i = startRow to startRow + 2:
            for j = startCol to startCol + 2:
                if board[i][j] != '.':
                    if board[i][j] in seen:
                        return false
                    seen.add(board[i][j])
    
    return true
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public boolean isValidSudoku(char[][] board) {
        // Check rows
        for (int i = 0; i < 9; i++) {
            Set<Character> seen = new HashSet<>();
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    if (seen.contains(board[i][j])) {
                        return false;
                    }
                    seen.add(board[i][j]);
                }
            }
        }
        
        // Check columns
        for (int j = 0; j < 9; j++) {
            Set<Character> seen = new HashSet<>();
            for (int i = 0; i < 9; i++) {
                if (board[i][j] != '.') {
                    if (seen.contains(board[i][j])) {
                        return false;
                    }
                    seen.add(board[i][j]);
                }
            }
        }
        
        // Check sub-boxes
        for (int box = 0; box < 9; box++) {
            Set<Character> seen = new HashSet<>();
            int startRow = (box / 3) * 3;
            int startCol = (box % 3) * 3;
            for (int i = startRow; i < startRow + 3; i++) {
                for (int j = startCol; j < startCol + 3; j++) {
                    if (board[i][j] != '.') {
                        if (seen.contains(board[i][j])) {
                            return false;
                        }
                        seen.add(board[i][j]);
                    }
                }
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
#include <vector>
#include <unordered_set>

class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        // Check rows
        for (int i = 0; i < 9; i++) {
            unordered_set<char> seen;
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    if (seen.find(board[i][j]) != seen.end()) {
                        return false;
                    }
                    seen.insert(board[i][j]);
                }
            }
        }
        
        // Check columns
        for (int j = 0; j < 9; j++) {
            unordered_set<char> seen;
            for (int i = 0; i < 9; i++) {
                if (board[i][j] != '.') {
                    if (seen.find(board[i][j]) != seen.end()) {
                        return false;
                    }
                    seen.insert(board[i][j]);
                }
            }
        }
        
        // Check sub-boxes
        for (int box = 0; box < 9; box++) {
            unordered_set<char> seen;
            int startRow = (box / 3) * 3;
            int startCol = (box % 3) * 3;
            for (int i = startRow; i < startRow + 3; i++) {
                for (int j = startCol; j < startCol + 3; j++) {
                    if (board[i][j] != '.') {
                        if (seen.find(board[i][j]) != seen.end()) {
                            return false;
                        }
                        seen.insert(board[i][j]);
                    }
                }
            }
        }
        
        return true;
    }
};

```

</div>

**Time Complexity:** O(1) - fixed 9x9 board, so O(81) = O(1)  
**Space Complexity:** O(1) - fixed size sets

## Approach 2: Single Pass with String Keys

### Explanation
Use a single pass through the board. For each cell, create unique string keys for row, column, and box constraints. Use a set to track all seen keys.

### Pseudocode
```
seen = new Set()
for i = 0 to 8:
    for j = 0 to 8:
        if board[i][j] != '.':
            num = board[i][j]
            rowKey = "row" + i + num
            colKey = "col" + j + num
            boxKey = "box" + (i/3) + (j/3) + num
            
            if rowKey in seen or colKey in seen or boxKey in seen:
                return false
            
            seen.add(rowKey)
            seen.add(colKey)
            seen.add(boxKey)

return true
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
import java.util.*;

class Solution {
    public boolean isValidSudoku(char[][] board) {
        Set<String> seen = new HashSet<>();
        
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    char num = board[i][j];
                    String rowKey = "row" + i + num;
                    String colKey = "col" + j + num;
                    String boxKey = "box" + (i / 3) + (j / 3) + num;
                    
                    if (seen.contains(rowKey) || seen.contains(colKey) || seen.contains(boxKey)) {
                        return false;
                    }
                    
                    seen.add(rowKey);
                    seen.add(colKey);
                    seen.add(boxKey);
                }
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
#include <vector>
#include <unordered_set>
#include <string>

class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        unordered_set<string> seen;
        
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    char num = board[i][j];
                    string rowKey = "row" + to_string(i) + num;
                    string colKey = "col" + to_string(j) + num;
                    string boxKey = "box" + to_string(i / 3) + to_string(j / 3) + num;
                    
                    if (seen.find(rowKey) != seen.end() || 
                        seen.find(colKey) != seen.end() || 
                        seen.find(boxKey) != seen.end()) {
                        return false;
                    }
                    
                    seen.insert(rowKey);
                    seen.insert(colKey);
                    seen.insert(boxKey);
                }
            }
        }
        
        return true;
    }
};

```

</div>

**Time Complexity:** O(1) - fixed 9x9 board  
**Space Complexity:** O(1) - fixed size set

## Approach 3: Bit Manipulation (Optimal)

### Explanation
Use bit masks to track which numbers have been seen in each row, column, and box. This is more memory efficient than sets.

### Pseudocode
```
rows = array of 9 integers (bit masks)
cols = array of 9 integers (bit masks)
boxes = array of 9 integers (bit masks)

for i = 0 to 8:
    for j = 0 to 8:
        if board[i][j] != '.':
            num = board[i][j] - '1'
            bit = 1 << num
            boxIndex = (i / 3) * 3 + (j / 3)
            
            if (rows[i] & bit) != 0 or (cols[j] & bit) != 0 or (boxes[boxIndex] & bit) != 0:
                return false
            
            rows[i] |= bit
            cols[j] |= bit
            boxes[boxIndex] |= bit

return true
```

### Java Code
<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; overflow-x: auto;">

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        int[] rows = new int[9];
        int[] cols = new int[9];
        int[] boxes = new int[9];
        
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    int num = board[i][j] - '1';
                    int bit = 1 << num;
                    int boxIndex = (i / 3) * 3 + (j / 3);
                    
                    if ((rows[i] & bit) != 0 || (cols[j] & bit) != 0 || (boxes[boxIndex] & bit) != 0) {
                        return false;
                    }
                    
                    rows[i] |= bit;
                    cols[j] |= bit;
                    boxes[boxIndex] |= bit;
                }
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
#include <vector>

class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<int> rows(9, 0);
        vector<int> cols(9, 0);
        vector<int> boxes(9, 0);
        
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    int num = board[i][j] - '1';
                    int bit = 1 << num;
                    int boxIndex = (i / 3) * 3 + (j / 3);
                    
                    if ((rows[i] & bit) != 0 || (cols[j] & bit) != 0 || (boxes[boxIndex] & bit) != 0) {
                        return false;
                    }
                    
                    rows[i] |= bit;
                    cols[j] |= bit;
                    boxes[boxIndex] |= bit;
                }
            }
        }
        
        return true;
    }
};

```

</div>

**Time Complexity:** O(1) - fixed 9x9 board  
**Space Complexity:** O(1) - fixed size arrays

## Comparison Table

| Approach | Time Complexity | Space Complexity | When to Use |
|----------|----------------|------------------|-------------|
| Brute Force (Three Separate Checks) | O(1) | O(1) | Clear and easy to understand. Three separate loops make the logic explicit. Good for learning |
| Single Pass with String Keys | O(1) | O(1) | More elegant single pass solution. Uses string concatenation. Good balance of readability and efficiency |
| Bit Manipulation (Optimal) | O(1) | O(1) | Most memory efficient using bit masks. Fastest in practice. Best for production code when performance matters most |

