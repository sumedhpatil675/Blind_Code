# Number of Islands

## Problem Description
Given an m x n 2D binary grid where '1' represents land and '0' represents water, count the number of islands using Breadth-First Search.

## Example Grid
```
  0 1 2 3 4
0 1 1 0 0 0
1 1 1 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1
```
In this grid, there are 3 islands.

## Approaches

### 1. Depth First Search (DFS)  ✅✅✅

- **Time complexity**: O(m*n)
- **Space complexity**: O(m*n)

Where m is the number of rows and n is the number of columns in the grid.

#### Python
```python
def numIslands(grid):
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    count = 0
    
    def dfs(r, c):
        # Check bounds and if current cell is land
        if (r < 0 or r >= rows or c < 0 or c >= cols or grid[r][c] == '0'):
            return
        
        # Mark as visited by changing to '0'
        grid[r][c] = '0'
        
        # Visit all adjacent cells
        dfs(r + 1, c)  # down
        dfs(r - 1, c)  # up
        dfs(r, c + 1)  # right
        dfs(r, c - 1)  # left
    
    # Iterate through the grid
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                count += 1
                dfs(r, c)  # Sink the island
    
    return count
```

#### JavaScript
```javascript
function numIslands(grid) {
    if (!grid || grid.length === 0) {
        return 0;
    }
    
    const rows = grid.length;
    const cols = grid[0].length;
    let count = 0;
    
    function dfs(r, c) {
        // Check bounds and if current cell is land
        if (r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] === '0') {
            return;
        }
        
        // Mark as visited by changing to '0'
        grid[r][c] = '0';
        
        // Visit all adjacent cells
        dfs(r + 1, c);  // down
        dfs(r - 1, c);  // up
        dfs(r, c + 1);  // right
        dfs(r, c - 1);  // left
    }
    
    // Iterate through the grid
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === '1') {
                count++;
                dfs(r, c);  // Sink the island
            }
        }
    }
    
    return count;
}
```

#### Java
```java
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    
    int rows = grid.length;
    int cols = grid[0].length;
    int count = 0;
    
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            if (grid[r][c] == '1') {
                count++;
                dfs(grid, r, c);
            }
        }
    }
    
    return count;
}

private void dfs(char[][] grid, int r, int c) {
    int rows = grid.length;
    int cols = grid[0].length;
    
    // Check bounds and if current cell is land
    if (r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] == '0') {
        return;
    }
    
    // Mark as visited by changing to '0'
    grid[r][c] = '0';
    
    // Visit all adjacent cells
    dfs(grid, r + 1, c);  // down
    dfs(grid, r - 1, c);  // up
    dfs(grid, r, c + 1);  // right
    dfs(grid, r, c - 1);  // left
}
```

### 2. Breadth First Search (BFS)
- **Time complexity**: O(m*n)
- **Space complexity**: O(min(m,n)) in the worst case

## Python Implementation
```python
from collections import deque
from typing import List

def numIslands(grid: List[List[str]]) -> int:
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    visit = set()
    islands = 0
    
    def bfs(r, c):
        q = deque()
        visit.add((r, c))
        q.append((r, c))
        
        while q:
            row, col = q.popleft()
            directions = [[1, 0], [-1, 0], [0, 1], [0, -1]]
            
            for dr, dc in directions:
                r, c = row + dr, col + dc
                if (r in range(rows) and 
                    c in range(cols) and 
                    grid[r][c] == "1" and 
                    (r, c) not in visit):
                    q.append((r, c))
                    visit.add((r, c))
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == "1" and (r, c) not in visit:
                bfs(r, c)
                islands += 1
    
    return islands
```
# Number of Islands - BFS Explanation

## Original Grid Example
```
  0 1 2 3 4
0 1 1 0 0 0
1 1 1 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1
```

## BFS Process Using Visit Set

```
Step 1: Start BFS from (0,0)
  0 1 2 3 4         Queue: [(0,0)]
0 v 1 0 0 0         Visit set: {(0,0)}
1 1 1 0 0 0         Islands: 1
2 0 0 1 0 0         v = visited
3 0 0 0 1 1

Step 2: Process (0,0), add neighbors
  0 1 2 3 4         Queue: [(0,1), (1,0)]
0 v v 0 0 0         Visit set: {(0,0), (0,1), (1,0)}
1 v 1 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1

Step 3: Process (0,1), add neighbor
  0 1 2 3 4         Queue: [(1,0), (1,1)]
0 v v 0 0 0         Visit set: {(0,0), (0,1), (1,0), (1,1)}
1 v v 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1

Step 4: Process (1,0), no new neighbors
  0 1 2 3 4         Queue: [(1,1)]
0 v v 0 0 0         Visit set: {(0,0), (0,1), (1,0), (1,1)}
1 v v 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1

Step 5: Process (1,1), queue empty
  0 1 2 3 4         Queue: []
0 v v 0 0 0         Visit set: {(0,0), (0,1), (1,0), (1,1)}
1 v v 0 0 0         First island fully explored
2 0 0 1 0 0
3 0 0 0 1 1
```

## Detailed BFS Execution Trace

```
+-------+----------------+--------------------+------------------+----------------+
| Step  | Current Cell   | Queue After        | Visit Set After  | Islands Count  |
+-------+----------------+--------------------+------------------+----------------+
| Start | -              | []                 | {}               | 0              |
| 1     | Find (0,0)='1' | [(0,0)]           | {(0,0)}          | 1              |
| 2     | (0,0)          | [(0,1), (1,0)]    | {(0,0),(0,1),    | 1              |
|       |                |                    |  (1,0)}          |                |
| 3     | (0,1)          | [(1,0), (1,1)]    | {(0,0),(0,1),    | 1              |
|       |                |                    |  (1,0),(1,1)}    |                |
| 4     | (1,0)          | [(1,1)]           | {(0,0),(0,1),    | 1              |
|       |                |                    |  (1,0),(1,1)}    |                |
| 5     | (1,1)          | []                | {(0,0),(0,1),    | 1              |
|       |                |                    |  (1,0),(1,1)}    |                |
| 6     | Find (2,2)='1' | [(2,2)]           | {(0,0),...,(2,2)}| 2              |
| 7     | (2,2)          | []                | {(0,0),...,(2,2)}| 2              |
| 8     | Find (3,3)='1' | [(3,3)]           | {(0,0),...,(3,3)}| 3              |
| 9     | (3,3)          | [(3,4)]           | {(0,0),...,(3,3),| 3              |
|       |                |                    |  (3,4)}          |                |
| 10    | (3,4)          | []                | {(0,0),...,(3,4)}| 3              |
| Final | -              | []                | 10 cells visited | 3              |
+-------+----------------+--------------------+------------------+----------------+
```

## Key Algorithm Steps in ASCII

```
Main Loop:
┌─────────────────────────────────┐
│ For each cell (r,c) in grid:    │
│  └─► If cell is land AND        │
│      not visited:               │
│       │                         │
│       └─► Start BFS from (r,c)  │
│           +1 island count       │
└─────────────────────────────────┘

BFS Function:
┌─────────────────────────────────┐
│ Add starting cell to visit set  │
│ Add starting cell to queue      │
│                                 │
│ While queue not empty:          │
│  └─► Get cell from queue        │
│      │                          │
│      └─► For each direction:    │
│           Check if neighbor is: │
│           1. Within bounds      │
│           2. Land cell ('1')    │
│           3. Not visited        │
│           │                     │
│           └─► Add to queue      │
│               Add to visit set  │
└─────────────────────────────────┘
```

## JavaScript Implementation
```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
function numIslands(grid) {
    if (!grid || grid.length === 0) {
        return 0;
    }
    
    const rows = grid.length;
    const cols = grid[0].length;
    const visit = new Set();
    let islands = 0;
    
    function bfs(r, c) {
        const q = [];
        const visitKey = `${r},${c}`;
        visit.add(visitKey);
        q.push([r, c]);
        
        while (q.length > 0) {
            const [row, col] = q.shift();
            const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
            
            for (const [dr, dc] of directions) {
                const newR = row + dr;
                const newC = col + dc;
                const newKey = `${newR},${newC}`;
                
                if (
                    newR >= 0 && newR < rows &&
                    newC >= 0 && newC < cols &&
                    grid[newR][newC] === "1" &&
                    !visit.has(newKey)
                ) {
                    q.push([newR, newC]);
                    visit.add(newKey);
                }
            }
        }
    }
    
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            const key = `${r},${c}`;
            if (grid[r][c] === "1" && !visit.has(key)) {
                bfs(r, c);
                islands++;
            }
        }
    }
    
    return islands;
}
```

## Java Implementation
```java
import java.util.*;

class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        
        int rows = grid.length;
        int cols = grid[0].length;
        Set<String> visit = new HashSet<>();
        int islands = 0;
        
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                String key = r + "," + c;
                if (grid[r][c] == '1' && !visit.contains(key)) {
                    bfs(grid, r, c, visit);
                    islands++;
                }
            }
        }
        
        return islands;
    }
    
    private void bfs(char[][] grid, int r, int c, Set<String> visit) {
        int rows = grid.length;
        int cols = grid[0].length;
        Queue<int[]> q = new LinkedList<>();
        
        visit.add(r + "," + c);
        q.offer(new int[]{r, c});
        
        while (!q.isEmpty()) {
            int[] pos = q.poll();
            int row = pos[0];
            int col = pos[1];
            
            int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
            
            for (int[] dir : directions) {
                int newR = row + dir[0];
                int newC = col + dir[1];
                String newKey = newR + "," + newC;
                
                if (
                    newR >= 0 && newR < rows &&
                    newC >= 0 && newC < cols &&
                    grid[newR][newC] == '1' &&
                    !visit.contains(newKey)
                ) {
                    q.offer(new int[]{newR, newC});
                    visit.add(newKey);
                }
            }
        }
    }
}
```


### 3. Union-Find (Disjoint Set Union)
- **Time complexity**: O(m*n)
- **Space complexity**: O(m*n)

#### Python
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.count = 0  # Count of distinct sets
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # Path compression
        return self.parent[x]
    
    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        
        if root_x == root_y:
            return
        
        # Union by rank
        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_x] > self.rank[root_y]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1
        
        self.count -= 1
    
    def add_island(self):
        self.count += 1

def numIslands(grid):
    if not grid or not grid[0]:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    uf = UnionFind(rows * cols)
    
    # Map 2D grid to 1D
    def get_index(r, c):
        return r * cols + c
    
    # First pass: Add islands and count them
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                uf.add_island()
    
    # Second pass: Union adjacent islands
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                index = get_index(r, c)
                
                # Check right and down neighbors
                if r + 1 < rows and grid[r + 1][c] == '1':
                    uf.union(index, get_index(r + 1, c))
                if c + 1 < cols and grid[r][c + 1] == '1':
                    uf.union(index, get_index(r, c + 1))
    
    return uf.count
```

#### JavaScript
```javascript
class UnionFind {
    constructor(n) {
        this.parent = Array.from({length: n}, (_, i) => i);
        this.rank = Array(n).fill(0);
        this.count = 0;  // Count of distinct sets
    }
    
    find(x) {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]);  // Path compression
        }
        return this.parent[x];
    }
    
    union(x, y) {
        const rootX = this.find(x);
        const rootY = this.find(y);
        
        if (rootX === rootY) {
            return;
        }
        
        // Union by rank
        if (this.rank[rootX] < this.rank[rootY]) {
            this.parent[rootX] = rootY;
        } else if (this.rank[rootX] > this.rank[rootY]) {
            this.parent[rootY] = rootX;
        } else {
            this.parent[rootY] = rootX;
            this.rank[rootX]++;
        }
        
        this.count--;
    }
    
    addIsland() {
        this.count++;
    }
}

function numIslands(grid) {
    if (!grid || grid.length === 0) {
        return 0;
    }
    
    const rows = grid.length;
    const cols = grid[0].length;
    const uf = new UnionFind(rows * cols);
    
    // Map 2D grid to 1D
    function getIndex(r, c) {
        return r * cols + c;
    }
    
    // First pass: Add islands and count them
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === '1') {
                uf.addIsland();
            }
        }
    }
    
    // Second pass: Union adjacent islands
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === '1') {
                const index = getIndex(r, c);
                
                // Check right and down neighbors
                if (r + 1 < rows && grid[r + 1][c] === '1') {
                    uf.union(index, getIndex(r + 1, c));
                }
                if (c + 1 < cols && grid[r][c + 1] === '1') {
                    uf.union(index, getIndex(r, c + 1));
                }
            }
        }
    }
    
    return uf.count;
}
```

#### Java
```java
class UnionFind {
    private int[] parent;
    private int[] rank;
    private int count;  // Count of distinct sets
    
    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
        count = 0;
    }
    
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);  // Path compression
        }
        return parent[x];
    }
    
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) {
            return;
        }
        
        // Union by rank
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
        
        count--;
    }
    
    public void addIsland() {
        count++;
    }
    
    public int getCount() {
        return count;
    }
}

public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    
    int rows = grid.length;
    int cols = grid[0].length;
    UnionFind uf = new UnionFind(rows * cols);
    
    // Map 2D grid to 1D
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            if (grid[r][c] == '1') {
                uf.addIsland();
            }
        }
    }
    
    // Second pass: Union adjacent islands
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            if (grid[r][c] == '1') {
                int index = r * cols + c;
                
                // Check right and down neighbors
                if (r + 1 < rows && grid[r + 1][c] == '1') {
                    uf.union(index, (r + 1) * cols + c);
                }
                if (c + 1 < cols && grid[r][c + 1] == '1') {
                    uf.union(index, r * cols + c + 1);
                }
            }
        }
    }
    
    return uf.getCount();
}
```
