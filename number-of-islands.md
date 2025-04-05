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

### 1. Depth First Search (DFS)
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

#### Python
```python
from collections import deque

def numIslands(grid):
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    count = 0
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                count += 1
                grid[r][c] = '0'  # Mark as visited
                
                # BFS to find the entire island
                queue = deque([(r, c)])
                while queue:
                    cur_r, cur_c = queue.popleft()
                    
                    # Check all 4 directions
                    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
                    for dr, dc in directions:
                        next_r, next_c = cur_r + dr, cur_c + dc
                        if (0 <= next_r < rows and 0 <= next_c < cols 
                            and grid[next_r][next_c] == '1'):
                            queue.append((next_r, next_c))
                            grid[next_r][next_c] = '0'  # Mark as visited
    
    return count
```

## BFS Algorithm Visualization

### First Island (BFS)
```
Step 1: Find first '1' at (0,0), add to queue
  0 1 2 3 4      Queue: [(0,0)]
0 1 1 0 0 0      Island count = 1
1 1 1 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1

Step 2: Process (0,0), mark as visited, add neighbors
  0 1 2 3 4      Queue: [(0,1), (1,0)]
0 X 1 0 0 0      X = visited
1 1 1 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1

Step 3: Process (0,1), mark as visited, add neighbors
  0 1 2 3 4      Queue: [(1,0), (1,1)]
0 X X 0 0 0
1 1 1 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1

Step 4: Process (1,0), mark as visited, add neighbors
  0 1 2 3 4      Queue: [(1,1)]
0 X X 0 0 0
1 X 1 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1

Step 5: Process (1,1), mark as visited
  0 1 2 3 4      Queue: []
0 X X 0 0 0
1 X X 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1
```

### Second Island (BFS)
```
Step 6: Find next '1' at (2,2), add to queue
  0 1 2 3 4      Queue: [(2,2)]
0 X X 0 0 0      Island count = 2
1 X X 0 0 0
2 0 0 1 0 0
3 0 0 0 1 1

Step 7: Process (2,2), mark as visited
  0 1 2 3 4      Queue: []
0 X X 0 0 0
1 X X 0 0 0
2 0 0 X 0 0
3 0 0 0 1 1
```

### Third Island (BFS)
```
Step 8: Find next '1' at (3,3), add to queue
  0 1 2 3 4      Queue: [(3,3)]
0 X X 0 0 0      Island count = 3
1 X X 0 0 0
2 0 0 X 0 0
3 0 0 0 1 1

Step 9: Process (3,3), mark as visited, add neighbor
  0 1 2 3 4      Queue: [(3,4)]
0 X X 0 0 0
1 X X 0 0 0
2 0 0 X 0 0
3 0 0 0 X 1

Step 10: Process (3,4), mark as visited
  0 1 2 3 4      Queue: []
0 X X 0 0 0
1 X X 0 0 0
2 0 0 X 0 0
3 0 0 0 X X
```

Final count = 3 islands

## BFS Execution Trace Table

```
+-------------------+-------------------+----------------+------------------+
| Current Position  | Queue Contents    | Islands Found  | Grid State       |
+-------------------+-------------------+----------------+------------------+
| Start             | []                | 0              | Original grid    |
| (0,0) found land  | [(0,0)]           | 1              | Original grid    |
| Process (0,0)     | [(0,1), (1,0)]    | 1              | (0,0) → X        |
| Process (0,1)     | [(1,0), (1,1)]    | 1              | (0,1) → X        |
| Process (1,0)     | [(1,1)]           | 1              | (1,0) → X        |
| Process (1,1)     | []                | 1              | (1,1) → X        |
| (2,2) found land  | [(2,2)]           | 2              | No change        |
| Process (2,2)     | []                | 2              | (2,2) → X        |
| (3,3) found land  | [(3,3)]           | 3              | No change        |
| Process (3,3)     | [(3,4)]           | 3              | (3,3) → X        |
| Process (3,4)     | []                | 3              | (3,4) → X        |
| Completed         | []                | 3              | All islands      |
|                   |                   |                | marked as visited|
+-------------------+-------------------+----------------+------------------+
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
    
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === '1') {
                count++;
                grid[r][c] = '0';  // Mark as visited
                
                // BFS to find the entire island
                const queue = [[r, c]];
                while (queue.length > 0) {
                    const [cur_r, cur_c] = queue.shift();
                    
                    // Check all 4 directions
                    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];
                    for (const [dr, dc] of directions) {
                        const next_r = cur_r + dr;
                        const next_c = cur_c + dc;
                        if (next_r >= 0 && next_r < rows && next_c >= 0 && next_c < cols 
                            && grid[next_r][next_c] === '1') {
                            queue.push([next_r, next_c]);
                            grid[next_r][next_c] = '0';  // Mark as visited
                        }
                    }
                }
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
                grid[r][c] = '0';  // Mark as visited
                
                // BFS to find the entire island
                Queue<int[]> queue = new LinkedList<>();
                queue.add(new int[]{r, c});
                
                while (!queue.isEmpty()) {
                    int[] current = queue.poll();
                    int cur_r = current[0];
                    int cur_c = current[1];
                    
                    // Check all 4 directions
                    int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
                    for (int[] dir : directions) {
                        int next_r = cur_r + dir[0];
                        int next_c = cur_c + dir[1];
                        
                        if (next_r >= 0 && next_r < rows && next_c >= 0 && next_c < cols 
                            && grid[next_r][next_c] == '1') {
                            queue.add(new int[]{next_r, next_c});
                            grid[next_r][next_c] = '0';  // Mark as visited
                        }
                    }
                }
            }
        }
    }
    
    return count;
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
