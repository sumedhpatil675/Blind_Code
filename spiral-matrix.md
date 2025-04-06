# Spiral Matrix

Given an m x n matrix, return all elements of the matrix in spiral order.

## Approaches

### 1. Layer by Layer (Simulation)
- **Time complexity**: O(m*n)
- **Space complexity**: O(1)

#### Python
```python
from typing import List

class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix:
            return []
            
        left, right = 0, len(matrix[0])
        top, bottom = 0, len(matrix)
        res = []
        
        while left < right and top < bottom:
            # left->right, traversing top-left to top-right
            for i in range(left, right):
                res.append(matrix[top][i])
            top += 1
            
            # top->bottom, traversing top-right to bottom-right
            for i in range(top, bottom):
                res.append(matrix[i][right-1])
            right -= 1
            
            if not (left < right and top < bottom):
                break
                
            # right->left, traversing from bottom-right to bottom-left
            for i in range(right-1, left-1, -1):
                res.append(matrix[bottom-1][i])
            bottom -= 1
            
            # bottom->top, traversing from bottom-left to top-left
            for i in range(bottom-1, top-1, -1):
                res.append(matrix[i][left])
            left += 1
            
        return res
```

#### JavaScript
```javascript
/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
function spiralOrder(matrix) {
    if (matrix.length === 0) return [];
    
    let left = 0;
    let right = matrix[0].length;
    let top = 0;
    let bottom = matrix.length;
    let result = [];
    
    while (left < right && top < bottom) {
        // left->right, traversing top-left to top-right
        for (let i = left; i < right; i++) {
            result.push(matrix[top][i]);
        }
        top++;
        
        // top->bottom, traversing top-right to bottom-right
        for (let i = top; i < bottom; i++) {
            result.push(matrix[i][right - 1]);
        }
        right--;
        
        if (!(left < right && top < bottom)) {
            break;
        }
        
        // right->left, traversing from bottom-right to bottom-left
        for (let i = right - 1; i >= left; i--) {
            result.push(matrix[bottom - 1][i]);
        }
        bottom--;
        
        // bottom->top, traversing from bottom-left to top-left
        for (let i = bottom - 1; i >= top; i--) {
            result.push(matrix[i][left]);
        }
        left++;
    }
    
    return result;
}
```

#### Java
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();
        
        if (matrix == null || matrix.length == 0) {
            return result;
        }
        
        int left = 0;
        int right = matrix[0].length;
        int top = 0;
        int bottom = matrix.length;
        
        while (left < right && top < bottom) {
            // left->right, traversing top-left to top-right
            for (int i = left; i < right; i++) {
                result.add(matrix[top][i]);
            }
            top++;
            
            // top->bottom, traversing top-right to bottom-right
            for (int i = top; i < bottom; i++) {
                result.add(matrix[i][right - 1]);
            }
            right--;
            
            if (!(left < right && top < bottom)) {
                break;
            }
            
            // right->left, traversing from bottom-right to bottom-left
            for (int i = right - 1; i >= left; i--) {
                result.add(matrix[bottom - 1][i]);
            }
            bottom--;
            
            // bottom->top, traversing from bottom-left to top-left
            for (int i = bottom - 1; i >= top; i--) {
                result.add(matrix[i][left]);
            }
            left++;
        }
        
        return result;
    }
}
```

### 2. Recursion
- **Time complexity**: O(m*n)
- **Space complexity**: O(m+n)

#### Python
```python
def spiralOrder(matrix):
    def spiral_coords(r1, c1, r2, c2):
        for c in range(c1, c2 + 1):
            yield r1, c
        for r in range(r1 + 1, r2 + 1):
            yield r, c2
        if r1 < r2 and c1 < c2:
            for c in range(c2 - 1, c1 - 1, -1):
                yield r2, c
            for r in range(r2 - 1, r1, -1):
                yield r, c1
    
    if not matrix:
        return []
    
    result = []
    rows, cols = len(matrix), len(matrix[0])
    r1, r2 = 0, rows - 1
    c1, c2 = 0, cols - 1
    
    while r1 <= r2 and c1 <= c2:
        for r, c in spiral_coords(r1, c1, r2, c2):
            result.append(matrix[r][c])
        r1 += 1
        r2 -= 1
        c1 += 1
        c2 -= 1
    
    return result
```

#### JavaScript
```javascript
function spiralOrder(matrix) {
    if (!matrix.length) {
        return [];
    }
    
    const result = [];
    const rows = matrix.length;
    const cols = matrix[0].length;
    
    function spiral(r1, c1, r2, c2) {
        // Base case
        if (r1 > r2 || c1 > c2) {
            return;
        }
        
        // Traverse right
        for (let j = c1; j <= c2; j++) {
            result.push(matrix[r1][j]);
        }
        
        // Traverse down
        for (let i = r1 + 1; i <= r2; i++) {
            result.push(matrix[i][c2]);
        }
        
        // Traverse left
        if (r1 < r2) {
            for (let j = c2 - 1; j >= c1; j--) {
                result.push(matrix[r2][j]);
            }
        }
        
        // Traverse up
        if (c1 < c2) {
            for (let i = r2 - 1; i > r1; i--) {
                result.push(matrix[i][c1]);
            }
        }
        
        // Recursive call for inner layer
        spiral(r1 + 1, c1 + 1, r2 - 1, c2 - 1);
    }
    
    spiral(0, 0, rows - 1, cols - 1);
    return result;
}
```

#### Java
```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> result = new ArrayList<>();
    if (matrix == null || matrix.length == 0) {
        return result;
    }
    
    int rows = matrix.length;
    int cols = matrix[0].length;
    
    spiral(matrix, 0, 0, rows - 1, cols - 1, result);
    return result;
}

private void spiral(int[][] matrix, int r1, int c1, int r2, int c2, List<Integer> result) {
    // Base case
    if (r1 > r2 || c1 > c2) {
        return;
    }
    
    // Traverse right
    for (int j = c1; j <= c2; j++) {
        result.add(matrix[r1][j]);
    }
    
    // Traverse down
    for (int i = r1 + 1; i <= r2; i++) {
        result.add(matrix[i][c2]);
    }
    
    // Traverse left
    if (r1 < r2) {
        for (int j = c2 - 1; j >= c1; j--) {
            result.add(matrix[r2][j]);
        }
    }
    
    // Traverse up
    if (c1 < c2) {
        for (int i = r2 - 1; i > r1; i--) {
            result.add(matrix[i][c1]);
        }
    }
    
    // Recursive call for inner layer
    spiral(matrix, r1 + 1, c1 + 1, r2 - 1, c2 - 1, result);
}
```

### 3. Direction Array
- **Time complexity**: O(m*n)
- **Space complexity**: O(1)

#### Python
```python
def spiralOrder(matrix):
    if not matrix:
        return []
    
    rows, cols = len(matrix), len(matrix[0])
    result = []
    
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # right, down, left, up
    dir_idx = 0
    
    r, c = 0, 0
    seen = [[False for _ in range(cols)] for _ in range(rows)]
    
    for _ in range(rows * cols):
        result.append(matrix[r][c])
        seen[r][c] = True
        
        # Calculate next position
        next_r, next_c = r + directions[dir_idx][0], c + directions[dir_idx][1]
        
        # Check if we need to change direction
        if (next_r < 0 or next_r >= rows or next_c < 0 or next_c >= cols or seen[next_r][next_c]):
            dir_idx = (dir_idx + 1) % 4
            next_r, next_c = r + directions[dir_idx][0], c + directions[dir_idx][1]
            
        r, c = next_r, next_c
    
    return result
```

#### JavaScript
```javascript
function spiralOrder(matrix) {
    if (!matrix.length) {
        return [];
    }
    
    const rows = matrix.length;
    const cols = matrix[0].length;
    const result = [];
    
    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];  // right, down, left, up
    let dirIdx = 0;
    
    let r = 0, c = 0;
    const seen = Array(rows).fill().map(() => Array(cols).fill(false));
    
    for (let i = 0; i < rows * cols; i++) {
        result.push(matrix[r][c]);
        seen[r][c] = true;
        
        // Calculate next position
        let nextR = r + directions[dirIdx][0];
        let nextC = c + directions[dirIdx][1];
        
        // Check if we need to change direction
        if (nextR < 0 || nextR >= rows || nextC < 0 || nextC >= cols || seen[nextR][nextC]) {
            dirIdx = (dirIdx + 1) % 4;
            nextR = r + directions[dirIdx][0];
            nextC = c + directions[dirIdx][1];
        }
        
        r = nextR;
        c = nextC;
    }
    
    return result;
}
```

#### Java
```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> result = new ArrayList<>();
    if (matrix == null || matrix.length == 0) {
        return result;
    }
    
    int rows = matrix.length;
    int cols = matrix[0].length;
    
    int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};  // right, down, left, up
    int dirIdx = 0;
    
    int r = 0, c = 0;
    boolean[][] seen = new boolean[rows][cols];
    
    for (int i = 0; i < rows * cols; i++) {
        result.add(matrix[r][c]);
        seen[r][c] = true;
        
        // Calculate next position
        int nextR = r + directions[dirIdx][0];
        int nextC = c + directions[dirIdx][1];
        
        // Check if we need to change direction
        if (nextR < 0 || nextR >= rows || nextC < 0 || nextC >= cols || seen[nextR][nextC]) {
            dirIdx = (dirIdx + 1) % 4;
            nextR = r + directions[dirIdx][0];
            nextC = c + directions[dirIdx][1];
        }
        
        r = nextR;
        c = nextC;
    }
    
    return result;
}
```
