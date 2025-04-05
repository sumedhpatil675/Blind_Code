# Word Search

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

## Approaches

### 1. Backtracking (Hash Set)

**Time complexity:** O(m * 4^n)  
**Space complexity:** O(n)  
where m is the number of cells in the board and n is the length of the word.

#### Python
```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        ROWS, COLS = len(board), len(board[0])
        path = set()
        
        def dfs(r, c, i):
            if i == len(word):
                return True
                
            if (min(r, c) < 0 or
                r >= ROWS or c >= COLS or
                word[i] != board[r][c] or
                (r, c) in path):
                return False
                
            path.add((r, c))
            res = (dfs(r + 1, c, i + 1) or
                   dfs(r - 1, c, i + 1) or
                   dfs(r, c + 1, i + 1) or
                   dfs(r, c - 1, i + 1))
            path.remove((r, c))
            return res
            
        for r in range(ROWS):
            for c in range(COLS):
                if dfs(r, c, 0):
                    return True
                    
        return False
```

#### JavaScript
```javascript
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
var exist = function(board, word) {
    const ROWS = board.length;
    const COLS = board[0].length;
    const path = new Set();
    
    function dfs(r, c, i) {
        if (i === word.length) {
            return true;
        }
        
        if (
            r < 0 || c < 0 || 
            r >= ROWS || c >= COLS || 
            word[i] !== board[r][c] || 
            path.has(`${r},${c}`)
        ) {
            return false;
        }
        
        path.add(`${r},${c}`);
        
        const res = (
            dfs(r + 1, c, i + 1) || 
            dfs(r - 1, c, i + 1) || 
            dfs(r, c + 1, i + 1) || 
            dfs(r, c - 1, i + 1)
        );
        
        path.delete(`${r},${c}`);
        return res;
    }
    
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (dfs(r, c, 0)) {
                return true;
            }
        }
    }
    
    return false;
};
```

#### Java
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int ROWS = board.length;
        int COLS = board[0].length;
        Set<String> path = new HashSet<>();
        
        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                if (dfs(board, word, r, c, 0, path)) {
                    return true;
                }
            }
        }
        
        return false;
    }
    
    private boolean dfs(char[][] board, String word, int r, int c, int i, Set<String> path) {
        if (i == word.length()) {
            return true;
        }
        
        if (r < 0 || c < 0 || 
            r >= board.length || c >= board[0].length || 
            word.charAt(i) != board[r][c] || 
            path.contains(r + "," + c)) {
            return false;
        }
        
        path.add(r + "," + c);
        
        boolean res = dfs(board, word, r + 1, c, i + 1, path) || 
                      dfs(board, word, r - 1, c, i + 1, path) || 
                      dfs(board, word, r, c + 1, i + 1, path) || 
                      dfs(board, word, r, c - 1, i + 1, path);
                      
        path.remove(r + "," + c);
        return res;
    }
}
```

### 2. Backtracking (Visited Array)

**Time complexity:** O(m * 4^n)  
**Space complexity:** O(m * n)  
where m is the number of cells in the board and n is the length of the word.

#### Python
```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        ROWS, COLS = len(board), len(board[0])
        visited = [[False for _ in range(COLS)] for _ in range(ROWS)]
        
        def dfs(r, c, i):
            if i == len(word):
                return True
                
            if (r < 0 or c < 0 or r >= ROWS or c >= COLS or
                word[i] != board[r][c] or visited[r][c]):
                return False
                
            visited[r][c] = True
            res = (dfs(r + 1, c, i + 1) or
                   dfs(r - 1, c, i + 1) or
                   dfs(r, c + 1, i + 1) or
                   dfs(r, c - 1, i + 1))
            visited[r][c] = False
            return res
            
        for r in range(ROWS):
            for c in range(COLS):
                if dfs(r, c, 0):
                    return True
                    
        return False
```

## Example with ASCII Diagrams

Let's consider a 3×4 board and the word "ABCCED":

```
    0   1   2   3
  +---+---+---+---+
0 | A | B | C | E |
  +---+---+---+---+
1 | S | F | C | S |
  +---+---+---+---+
2 | A | D | E | E |
  +---+---+---+---+
```

### Illustration of DFS Steps

Let's trace through the algorithm looking for the word "ABCCED":

1. Start at each cell and try to find the word starting with 'A'.
2. For this example, we'll start at (0,0) which contains 'A'.

#### Step 1: Start DFS from board[0][0] = 'A'

```
    0   1   2   3
  +---+---+---+---+
0 | A*| B | C | E |  * Current position
  +---+---+---+---+
1 | S | F | C | S |
  +---+---+---+---+
2 | A | D | E | E |
  +---+---+---+---+

visited = [
  [True, False, False, False],
  [False, False, False, False],
  [False, False, False, False]
]

i = 0, word[i] = 'A', matches board[0][0]
Mark visited[0][0] = True
```

#### Step 2: Move to board[0][1] = 'B'

```
    0   1   2   3
  +---+---+---+---+
0 | A | B*| C | E |  * Current position
  +---+---+---+---+
1 | S | F | C | S |
  +---+---+---+---+
2 | A | D | E | E |
  +---+---+---+---+

visited = [
  [True, True, False, False],
  [False, False, False, False],
  [False, False, False, False]
]

i = 1, word[i] = 'B', matches board[0][1]
Mark visited[0][1] = True
```

#### Step 3: Move to board[0][2] = 'C'

```
    0   1   2   3
  +---+---+---+---+
0 | A | B | C*| E |  * Current position
  +---+---+---+---+
1 | S | F | C | S |
  +---+---+---+---+
2 | A | D | E | E |
  +---+---+---+---+

visited = [
  [True, True, True, False],
  [False, False, False, False],
  [False, False, False, False]
]

i = 2, word[i] = 'C', matches board[0][2]
Mark visited[0][2] = True
```

#### Step 4: Move to board[1][2] = 'C'

```
    0   1   2   3
  +---+---+---+---+
0 | A | B | C | E |
  +---+---+---+---+
1 | S | F | C*| S |  * Current position
  +---+---+---+---+
2 | A | D | E | E |
  +---+---+---+---+

visited = [
  [True, True, True, False],
  [False, False, True, False],
  [False, False, False, False]
]

i = 3, word[i] = 'C', matches board[1][2]
Mark visited[1][2] = True
```

#### Step 5: Move to board[2][2] = 'E'

```
    0   1   2   3
  +---+---+---+---+
0 | A | B | C | E |
  +---+---+---+---+
1 | S | F | C | S |
  +---+---+---+---+
2 | A | D | E*| E |  * Current position
  +---+---+---+---+

visited = [
  [True, True, True, False],
  [False, False, True, False],
  [False, False, True, False]
]

i = 4, word[i] = 'E', matches board[2][2]
Mark visited[2][2] = True
```

#### Step 6: Move to board[2][1] = 'D'

```
    0   1   2   3
  +---+---+---+---+
0 | A | B | C | E |
  +---+---+---+---+
1 | S | F | C | S |
  +---+---+---+---+
2 | A | D*| E | E |  * Current position
  +---+---+---+---+

visited = [
  [True, True, True, False],
  [False, False, True, False],
  [False, True, True, False]
]

i = 5, word[i] = 'D', matches board[2][1]
Mark visited[2][1] = True
```

#### Step 7: Complete!

We've found all characters of "ABCCED", so the function returns `True`.

The path we took:
```
    0   1   2   3
  +---+---+---+---+
0 | 1 | 2 | 3 | - |  Numbers show our path
  +---+---+---+---+
1 | - | - | 4 | - |
  +---+---+---+---+
2 | - | 6 | 5 | - |
  +---+---+---+---+
```

## Backtracking Process

The DFS function uses backtracking:

1. If we've found all characters of the word, return `True`
2. If we go out of bounds, current character doesn't match, or we've already visited this cell, return `False`
3. Otherwise, mark current cell as visited
4. Recursively explore all four adjacent cells (up, down, left, right)
5. If any of these paths lead to success, return `True`
6. Before returning, mark the current cell as unvisited (backtrack)

### Visualizing the visited matrix during backtracking:

When we explore a path that doesn't work, we backtrack and reset the visited status:

```
After trying an unsuccessful path:

visited = [
  [True, True, True, False],
  [False, False, True, False],
  [False, True, True, False]
]

During backtracking, we set visited[2][1] = False:

visited = [
  [True, True, True, False],
  [False, False, True, False],
  [False, False, True, False]
]

And continue backtracking as needed...
```

This backtracking allows us to reuse cells for different paths, as long as we don't reuse the same cell within a single path.


#### JavaScript
```javascript
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
var exist = function(board, word) {
    const ROWS = board.length;
    const COLS = board[0].length;
    const visited = Array(ROWS).fill().map(() => Array(COLS).fill(false));
    
    function dfs(r, c, i) {
        if (i === word.length) {
            return true;
        }
        
        if (
            r < 0 || c < 0 || 
            r >= ROWS || c >= COLS || 
            word[i] !== board[r][c] || 
            visited[r][c]
        ) {
            return false;
        }
        
        visited[r][c] = true;
        
        const res = (
            dfs(r + 1, c, i + 1) || 
            dfs(r - 1, c, i + 1) || 
            dfs(r, c + 1, i + 1) || 
            dfs(r, c - 1, i + 1)
        );
        
        visited[r][c] = false;
        return res;
    }
    
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (dfs(r, c, 0)) {
                return true;
            }
        }
    }
    
    return false;
};
```

#### Java
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int ROWS = board.length;
        int COLS = board[0].length;
        boolean[][] visited = new boolean[ROWS][COLS];
        
        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                if (dfs(board, word, r, c, 0, visited)) {
                    return true;
                }
            }
        }
        
        return false;
    }
    
    private boolean dfs(char[][] board, String word, int r, int c, int i, boolean[][] visited) {
        if (i == word.length()) {
            return true;
        }
        
        if (r < 0 || c < 0 || 
            r >= board.length || c >= board[0].length || 
            word.charAt(i) != board[r][c] || 
            visited[r][c]) {
            return false;
        }
        
        visited[r][c] = true;
        
        boolean res = dfs(board, word, r + 1, c, i + 1, visited) || 
                      dfs(board, word, r - 1, c, i + 1, visited) || 
                      dfs(board, word, r, c + 1, i + 1, visited) || 
                      dfs(board, word, r, c - 1, i + 1, visited);
                      
        visited[r][c] = false;
        return res;
    }
}
```

### 3. Backtracking (Optimal)

**Time complexity:** O(m * 4^n)  
**Space complexity:** O(n)  
where m is the number of cells in the board and n is the length of the word.

#### Python
```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        ROWS, COLS = len(board), len(board[0])
        
        def dfs(r, c, i):
            if i == len(word):
                return True
                
            if (r < 0 or c < 0 or r >= ROWS or c >= COLS or
                word[i] != board[r][c] or board[r][c] == '#'):
                return False
                
            board[r][c] = '#'
            res = (dfs(r + 1, c, i + 1) or
                   dfs(r - 1, c, i + 1) or
                   dfs(r, c + 1, i + 1) or
                   dfs(r, c - 1, i + 1))
            board[r][c] = word[i]
            return res
            
        for r in range(ROWS):
            for c in range(COLS):
                if dfs(r, c, 0):
                    return True
                    
        return False
```

#### JavaScript
```javascript
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
var exist = function(board, word) {
    const ROWS = board.length;
    const COLS = board[0].length;
    
    function dfs(r, c, i) {
        if (i === word.length) {
            return true;
        }
        
        if (
            r < 0 || c < 0 || 
            r >= ROWS || c >= COLS || 
            word[i] !== board[r][c] || 
            board[r][c] === '#'
        ) {
            return false;
        }
        
        const temp = board[r][c];
        board[r][c] = '#'; // Mark as visited
        
        const res = (
            dfs(r + 1, c, i + 1) || 
            dfs(r - 1, c, i + 1) || 
            dfs(r, c + 1, i + 1) || 
            dfs(r, c - 1, i + 1)
        );
        
        board[r][c] = temp; // Restore original value
        return res;
    }
    
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (dfs(r, c, 0)) {
                return true;
            }
        }
    }
    
    return false;
};
```

#### Java
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int ROWS = board.length;
        int COLS = board[0].length;
        
        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                if (dfs(board, word, r, c, 0)) {
                    return true;
                }
            }
        }
        
        return false;
    }
    
    private boolean dfs(char[][] board, String word, int r, int c, int i) {
        if (i == word.length()) {
            return true;
        }
        
        if (r < 0 || c < 0 || 
            r >= board.length || c >= board[0].length || 
            word.charAt(i) != board[r][c] || 
            board[r][c] == '#') {
            return false;
        }
        
        char temp = board[r][c];
        board[r][c] = '#'; // Mark as visited
        
        boolean res = dfs(board, word, r + 1, c, i + 1) || 
                      dfs(board, word, r - 1, c, i + 1) || 
                      dfs(board, word, r, c + 1, i + 1) || 
                      dfs(board, word, r, c - 1, i + 1);
                      
        board[r][c] = temp; // Restore original value
        return res;
    }
}
```
