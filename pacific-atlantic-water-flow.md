# Pacific Atlantic Water Flow

There is an m x n rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an m x n integer matrix heights where heights[r][c] represents the height above sea level of the cell at coordinate (r, c).

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a list of grid coordinates result where result[i] = [ri, ci] denotes that rain water can flow from cell (ri, ci) to both the Pacific and Atlantic oceans.

## Approaches

### 1. Brute Force (Backtracking)

**Time complexity:** O(m * n * 4^(m*n))  
**Space complexity:** O(m * n)  
where m is the number of rows and n is the number of columns.

#### Python
```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        ROWS, COLS = len(heights), len(heights[0])
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        pacific = atlantic = False
        
        def dfs(r, c, prevVal):
            nonlocal pacific, atlantic
            
            if r < 0 or c < 0:
                pacific = True
                return
                
            if r >= ROWS or c >= COLS:
                atlantic = True
                return
                
            if heights[r][c] > prevVal:
                return
                
            tmp = heights[r][c]
            heights[r][c] = float('inf')
            
            for dx, dy in directions:
                dfs(r + dx, c + dy, tmp)
                if pacific and atlantic:
                    break
                    
            heights[r][c] = tmp
            
        res = []
        for r in range(ROWS):
            for c in range(COLS):
                pacific = False
                atlantic = False
                dfs(r, c, float('inf'))
                if pacific and atlantic:
                    res.append([r, c])
                    
        return res
```

#### JavaScript
```javascript
/**
 * @param {number[][]} heights
 * @return {number[][]}
 */
var pacificAtlantic = function(heights) {
    const ROWS = heights.length;
    const COLS = heights[0].length;
    const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
    const result = [];
    
    function dfs(r, c, prevVal) {
        let pacific = false;
        let atlantic = false;
        
        if (r < 0 || c < 0) {
            pacific = true;
        }
        
        if (r >= ROWS || c >= COLS) {
            atlantic = true;
        }
        
        if (pacific && atlantic) {
            return { pacific, atlantic };
        }
        
        if (r < 0 || c < 0 || r >= ROWS || c >= COLS || heights[r][c] > prevVal) {
            return { pacific, atlantic };
        }
        
        const temp = heights[r][c];
        heights[r][c] = Infinity; // Mark as visited
        
        for (const [dr, dc] of directions) {
            const result = dfs(r + dr, c + dc, temp);
            pacific = pacific || result.pacific;
            atlantic = atlantic || result.atlantic;
            
            if (pacific && atlantic) {
                break;
            }
        }
        
        heights[r][c] = temp; // Restore original height
        return { pacific, atlantic };
    }
    
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            const { pacific, atlantic } = dfs(r, c, Infinity);
            if (pacific && atlantic) {
                result.push([r, c]);
            }
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    private int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        int ROWS = heights.length;
        int COLS = heights[0].length;
        List<List<Integer>> result = new ArrayList<>();
        
        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                boolean[] canReach = new boolean[2]; // [pacific, atlantic]
                
                if (dfs(heights, r, c, Integer.MAX_VALUE, canReach, new boolean[ROWS][COLS])) {
                    List<Integer> cell = new ArrayList<>();
                    cell.add(r);
                    cell.add(c);
                    result.add(cell);
                }
            }
        }
        
        return result;
    }
    
    private boolean dfs(int[][] heights, int r, int c, int prevHeight, boolean[] canReach, boolean[][] visited) {
        int ROWS = heights.length;
        int COLS = heights[0].length;
        
        if (r < 0 || c < 0) {
            canReach[0] = true; // Pacific
            return canReach[0] && canReach[1];
        }
        
        if (r >= ROWS || c >= COLS) {
            canReach[1] = true; // Atlantic
            return canReach[0] && canReach[1];
        }
        
        if (heights[r][c] > prevHeight || visited[r][c]) {
            return canReach[0] && canReach[1];
        }
        
        if (canReach[0] && canReach[1]) {
            return true;
        }
        
        visited[r][c] = true;
        int currentHeight = heights[r][c];
        
        for (int[] dir : directions) {
            dfs(heights, r + dir[0], c + dir[1], currentHeight, canReach, visited);
            if (canReach[0] && canReach[1]) {
                return true;
            }
        }
        
        visited[r][c] = false;
        return canReach[0] && canReach[1];
    }
}
```

### 2. Depth First Search ✅✅✅

**Time complexity:** O(m * n)  
**Space complexity:** O(m * n)  
where m is the number of rows and n is the number of columns.

#### Python
```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        ROWS, COLS = len(heights), len(heights[0])
        pac, atl = set(), set()
        
        def dfs(r, c, visit, prevHeight):
            if ((r, c) in visit or
                r < 0 or c < 0 or
                r == ROWS or c == COLS or
                heights[r][c] < prevHeight
               ):
                return
                
            visit.add((r, c))
            dfs(r + 1, c, visit, heights[r][c])
            dfs(r - 1, c, visit, heights[r][c])
            dfs(r, c + 1, visit, heights[r][c])
            dfs(r, c - 1, visit, heights[r][c])
            
        for c in range(COLS):
            dfs(0, c, pac, heights[0][c])
            dfs(ROWS - 1, c, atl, heights[ROWS - 1][c])
            
        for r in range(ROWS):
            dfs(r, 0, pac, heights[r][0])
            dfs(r, COLS - 1, atl, heights[r][COLS - 1])
            
        res = []
        for r in range(ROWS):
            for c in range(COLS):
                if (r, c) in pac and (r, c) in atl:
                    res.append([r, c])
                    
        return res
```
# Pacific Atlantic Water Flow - Visual Explanation

```
                  Pacific Ocean
                  ↓ ↓ ↓ ↓ ↓ ↓
                 +-----------+
                →|           |←
                →|           |←
                →|  Island   |←  Atlantic Ocean
                →|  Heights  |←
                →|           |←
                 +-----------+
                  ↑ ↑ ↑ ↑ ↑ ↑
                  Atlantic Ocean
```

## Problem Overview
- We have an island represented by a matrix of heights
- Pacific Ocean borders the top and left edges
- Atlantic Ocean borders the bottom and right edges
- Water flows from higher to equal or lower heights
- Find cells where water can flow to BOTH oceans

## Algorithm: Reverse DFS Approach

```
   Pacific Start Points      Atlantic Start Points
   ↓ ↓ ↓ ↓ ↓ ↓              ↑ ↑ ↑ ↑ ↑ ↑
  +---------------+         +---------------+
 →|S S S S S S S S|        |S S S S S S S S|←
 →|S              |        |              S|←
 →|S              |        |              S|←
 →|S              |        |              S|←
 →|S              |        |              S|←
  +---------------+         +---------------+
```

## How It Works

1. Create two sets: `pac` and `atl` to track reachable cells
2. Run DFS from Pacific borders (top and left edges)
   - Add cells to `pac` set where water can flow to Pacific
3. Run DFS from Atlantic borders (bottom and right edges)
   - Add cells to `atl` set where water can flow to Atlantic
4. Find the intersection of both sets

## DFS Logic (Reverse Flow)

```
DFS(r, c, visit, prevHeight):
  if ((r,c) in visit OR r<0 OR c<0 OR r==ROWS OR c==COLS OR heights[r][c]<prevHeight):
    return
  
  add (r,c) to visit set
  DFS(r+1, c, visit, heights[r][c])  # Down
  DFS(r-1, c, visit, heights[r][c])  # Up
  DFS(r, c+1, visit, heights[r][c])  # Right
  DFS(r, c-1, visit, heights[r][c])  # Left
```

## Why Reverse Flow?

```
Forward (Harder):             Reverse (Easier):
? ? ? ? ?                     ↑ ↑ ↑ ↑ ↑
? ? ? ? ?                     ↑ ↑ ? ? ↑
? ? ? ? ?  Atlantic Ocean     ↑ ? ? ? ↑  Atlantic Ocean
? ? ? ? ? →→→→→→               ? ? ? ↑ ↑ →→→→→→
```

- Forward approach: need to check all possible paths from each cell
- Reverse approach: just work backward from ocean edges

## Final Result Process

```
For every cell (r,c) in the grid:
  if (r,c) in pac AND (r,c) in atl:
    add [r,c] to result list
```

Example visualization (hypothetical 5×5 grid):

```
Pacific Reachable     Atlantic Reachable    Result (Intersection)
P P P P .             . . A A A             . . A A .
P P P . .             . A A A A             . A A . .
P P . . .             A A A A A             A A . . .
P . . . .             A A A A A             . . . . .
. . . . .             A A A A A             . . . . .

P = Cells that can flow to Pacific
A = Cells that can flow to Atlantic
R = Result cells (in both sets)
```

The algorithm returns coordinates of all 'R' cells.

The algorithm finds all cells that can reach BOTH oceans by exploring from the oceans inward and finding the overlap.


#### JavaScript
```javascript
/**
 * @param {number[][]} heights
 * @return {number[][]}
 */
var pacificAtlantic = function(heights) {
    const ROWS = heights.length;
    const COLS = heights[0].length;
    const pacific = new Set();
    const atlantic = new Set();
    const result = [];
    
    function dfs(r, c, visit, prevHeight) {
        const key = `${r},${c}`;
        
        if (
            visit.has(key) || 
            r < 0 || c < 0 || 
            r >= ROWS || c >= COLS || 
            heights[r][c] < prevHeight
        ) {
            return;
        }
        
        visit.add(key);
        
        dfs(r + 1, c, visit, heights[r][c]);
        dfs(r - 1, c, visit, heights[r][c]);
        dfs(r, c + 1, visit, heights[r][c]);
        dfs(r, c - 1, visit, heights[r][c]);
    }
    
    // Start DFS from Pacific edges
    for (let c = 0; c < COLS; c++) {
        dfs(0, c, pacific, heights[0][c]);
        dfs(ROWS - 1, c, atlantic, heights[ROWS - 1][c]);
    }
    
    for (let r = 0; r < ROWS; r++) {
        dfs(r, 0, pacific, heights[r][0]);
        dfs(r, COLS - 1, atlantic, heights[r][COLS - 1]);
    }
    
    // Find cells that can reach both oceans
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            const key = `${r},${c}`;
            if (pacific.has(key) && atlantic.has(key)) {
                result.push([r, c]);
            }
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    private int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        if (heights == null || heights.length == 0 || heights[0].length == 0) {
            return new ArrayList<>();
        }
        
        int ROWS = heights.length;
        int COLS = heights[0].length;
        Set<String> pacific = new HashSet<>();
        Set<String> atlantic = new HashSet<>();
        List<List<Integer>> result = new ArrayList<>();
        
        // DFS from Pacific edges
        for (int c = 0; c < COLS; c++) {
            dfs(heights, 0, c, pacific, heights[0][c]);
            dfs(heights, ROWS - 1, c, atlantic, heights[ROWS - 1][c]);
        }
        
        for (int r = 0; r < ROWS; r++) {
            dfs(heights, r, 0, pacific, heights[r][0]);
            dfs(heights, r, COLS - 1, atlantic, heights[r][COLS - 1]);
        }
        
        // Find cells that can reach both oceans
        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                String key = r + "," + c;
                if (pacific.contains(key) && atlantic.contains(key)) {
                    List<Integer> cell = new ArrayList<>();
                    cell.add(r);
                    cell.add(c);
                    result.add(cell);
                }
            }
        }
        
        return result;
    }
    
    private void dfs(int[][] heights, int r, int c, Set<String> visited, int prevHeight) {
        String key = r + "," + c;
        
        if (
            visited.contains(key) || 
            r < 0 || c < 0 || 
            r >= heights.length || c >= heights[0].length || 
            heights[r][c] < prevHeight
        ) {
            return;
        }
        
        visited.add(key);
        
        for (int[] dir : directions) {
            dfs(heights, r + dir[0], c + dir[1], visited, heights[r][c]);
        }
    }
}
```

### 3. Breadth First Search

**Time complexity:** O(m * n)  
**Space complexity:** O(m * n)  
where m is the number of rows and n is the number of columns.

#### Python
```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        ROWS, COLS = len(heights), len(heights[0])
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        
        pac = [[False] * COLS for _ in range(ROWS)]
        atl = [[False] * COLS for _ in range(ROWS)]
        
        def bfs(source, ocean):
            q = deque(source)
            
            while q:
                r, c = q.popleft()
                ocean[r][c] = True
                
                for dr, dc in directions:
                    nr, nc = r + dr, c + dc
                    
                    if (0 <= nr < ROWS and 0 <= nc < COLS and
                        not ocean[nr][nc] and
                        heights[nr][nc] >= heights[r][c]
                       ):
                        q.append((nr, nc))
                        
        pacific = []
        atlantic = []
        
        for c in range(COLS):
            pacific.append((0, c))
            atlantic.append((ROWS - 1, c))
            
        for r in range(ROWS):
            pacific.append((r, 0))
            atlantic.append((r, COLS - 1))
            
        bfs(pacific, pac)
        bfs(atlantic, atl)
        
        res = []
        for r in range(ROWS):
            for c in range(COLS):
                if pac[r][c] and atl[r][c]:
                    res.append([r, c])
                    
        return res
```

#### JavaScript
```javascript
/**
 * @param {number[][]} heights
 * @return {number[][]}
 */
var pacificAtlantic = function(heights) {
    const ROWS = heights.length;
    const COLS = heights[0].length;
    const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
    const result = [];
    
    // Initialize ocean reachability matrices
    const pacific = Array(ROWS).fill().map(() => Array(COLS).fill(false));
    const atlantic = Array(ROWS).fill().map(() => Array(COLS).fill(false));
    
    function bfs(sources, ocean) {
        const queue = [...sources];
        
        while (queue.length > 0) {
            const [r, c] = queue.shift();
            ocean[r][c] = true;
            
            for (const [dr, dc] of directions) {
                const nr = r + dr;
                const nc = c + dc;
                
                if (
                    nr >= 0 && nc >= 0 && 
                    nr < ROWS && nc < COLS && 
                    !ocean[nr][nc] && 
                    heights[nr][nc] >= heights[r][c]
                ) {
                    queue.push([nr, nc]);
                }
            }
        }
    }
    
    // Initialize edge cells
    const pacificSources = [];
    const atlanticSources = [];
    
    for (let c = 0; c < COLS; c++) {
        pacificSources.push([0, c]);
        atlanticSources.push([ROWS - 1, c]);
    }
    
    for (let r = 0; r < ROWS; r++) {
        pacificSources.push([r, 0]);
        atlanticSources.push([r, COLS - 1]);
    }
    
    bfs(pacificSources, pacific);
    bfs(atlanticSources, atlantic);
    
    // Find cells that can reach both oceans
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (pacific[r][c] && atlantic[r][c]) {
                result.push([r, c]);
            }
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    private int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        if (heights == null || heights.length == 0 || heights[0].length == 0) {
            return new ArrayList<>();
        }
        
        int ROWS = heights.length;
        int COLS = heights[0].length;
        boolean[][] pacific = new boolean[ROWS][COLS];
        boolean[][] atlantic = new boolean[ROWS][COLS];
        List<List<Integer>> result = new ArrayList<>();
        
        // Initialize edge cells
        List<int[]> pacificQueue = new ArrayList<>();
        List<int[]> atlanticQueue = new ArrayList<>();
        
        for (int c = 0; c < COLS; c++) {
            pacificQueue.add(new int[]{0, c});
            atlanticQueue.add(new int[]{ROWS - 1, c});
        }
        
        for (int r = 0; r < ROWS; r++) {
            pacificQueue.add(new int[]{r, 0});
            atlanticQueue.add(new int[]{r, COLS - 1});
        }
        
        bfs(heights, pacificQueue, pacific);
        bfs(heights, atlanticQueue, atlantic);
        
        // Find cells that can reach both oceans
        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                if (pacific[r][c] && atlantic[r][c]) {
                    List<Integer> cell = new ArrayList<>();
                    cell.add(r);
                    cell.add(c);
                    result.add(cell);
                }
            }
        }
        
        return result;
    }
    
    private void bfs(int[][] heights, List<int[]> queue, boolean[][] visited) {
        int ROWS = heights.length;
        int COLS = heights[0].length;
        Queue<int[]> q = new LinkedList<>(queue);
        
        for (int[] cell : queue) {
            visited[cell[0]][cell[1]] = true;
        }
        
        while (!q.isEmpty()) {
            int[] curr = q.poll();
            int r = curr[0];
            int c = curr[1];
            
            for (int[] dir : directions) {
                int nr = r + dir[0];
                int nc = c + dir[1];
                
                if (
                    nr >= 0 && nc >= 0 && 
                    nr < ROWS && nc < COLS && 
                    !visited[nr][nc] && 
                    heights[nr][nc] >= heights[r][c]
                ) {
                    visited[nr][nc] = true;
                    q.offer(new int[]{nr, nc});
                }
            }
        }
    }
}
```
