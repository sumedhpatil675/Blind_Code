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