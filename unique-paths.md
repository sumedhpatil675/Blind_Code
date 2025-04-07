# Unique Paths

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

## Approaches

### 1. Recursion
- **Time complexity**: O(2^(m+n))
- **Space complexity**: O(m+n)

#### Python
```python
def uniquePaths(m, n):
    def dfs(i, j):
        if i == (m - 1) and j == (n - 1):
            return 1
        if i >= m or j >= n:
            return 0
        return dfs(i, j + 1) + dfs(i + 1, j)
    
    return dfs(0, 0)
```

#### JavaScript
```javascript
function uniquePaths(m, n) {
    function dfs(i, j) {
        if (i === (m - 1) && j === (n - 1)) {
            return 1;
        }
        if (i >= m || j >= n) {
            return 0;
        }
        return dfs(i, j + 1) + dfs(i + 1, j);
    }
    
    return dfs(0, 0);
}
```

#### Java
```java
public int uniquePaths(int m, int n) {
    return dfs(0, 0, m, n);
}

private int dfs(int i, int j, int m, int n) {
    if (i == (m - 1) && j == (n - 1)) {
        return 1;
    }
    if (i >= m || j >= n) {
        return 0;
    }
    return dfs(i, j + 1, m, n) + dfs(i + 1, j, m, n);
}
```

### 2. Dynamic Programming (Top-Down)
- **Time complexity**: O(m*n)
- **Space complexity**: O(m*n)

#### Python
```python
def uniquePaths(m, n):
    memo = [[-1] * n for _ in range(m)]
    
    def dfs(i, j):
        if i == (m - 1) and j == (n - 1):
            return 1
        if i >= m or j >= n:
            return 0
        
        if memo[i][j] != -1:
            return memo[i][j]
        
        memo[i][j] = dfs(i, j + 1) + dfs(i + 1, j)
        return memo[i][j]
    
    return dfs(0, 0)
```

#### JavaScript
```javascript
function uniquePaths(m, n) {
    const memo = Array(m).fill().map(() => Array(n).fill(-1));
    
    function dfs(i, j) {
        if (i === (m - 1) && j === (n - 1)) {
            return 1;
        }
        if (i >= m || j >= n) {
            return 0;
        }
        
        if (memo[i][j] !== -1) {
            return memo[i][j];
        }
        
        memo[i][j] = dfs(i, j + 1) + dfs(i + 1, j);
        return memo[i][j];
    }
    
    return dfs(0, 0);
}
```

#### Java
```java
public int uniquePaths(int m, int n) {
    int[][] memo = new int[m][n];
    for (int i = 0; i < m; i++) {
        Arrays.fill(memo[i], -1);
    }
    return dfs(0, 0, m, n, memo);
}

private int dfs(int i, int j, int m, int n, int[][] memo) {
    if (i == (m - 1) && j == (n - 1)) {
        return 1;
    }
    if (i >= m || j >= n) {
        return 0;
    }
    
    if (memo[i][j] != -1) {
        return memo[i][j];
    }
    
    memo[i][j] = dfs(i, j + 1, m, n, memo) + dfs(i + 1, j, m, n, memo);
    return memo[i][j];
}
```

### 3. Dynamic Programming (Bottom-Up) ✅✅✅

- **Time complexity**: O(m*n)
- **Space complexity**: O(m*n)

#### Python Implementation

```python
def uniquePaths(m: int, n: int) -> int:
    # Create a 2D DP array with dimensions m x n filled with 1s
    dp = [[1] * n for _ in range(m)]
    
    # Fill the DP array: each cell contains the sum of the cell above and to the left
    for i in range(1, m):  # row
        for j in range(1, n):  # column
            dp[i][j] = dp[i][j-1] + dp[i-1][j]  # left + top
    
    # Return the bottom-right cell which contains the answer
    return dp[m-1][n-1]
```

### Explanation with DP Table (3×4 Grid Example)

Let's trace through the algorithm with a 3×4 grid (m=3, n=4).

#### Initial DP Table

When we initialize the dp table, all cells are filled with 1s. The first row and first column will always remain 1 because there is only one way to reach any cell in the first row (moving right) or first column (moving down).

```
    j=0 j=1 j=2 j=3
    +---+---+---+---+
i=0 | 1 | 1 | 1 | 1 |  First row remains 1
    +---+---+---+---+
i=1 | 1 |   |   |   |  First column 
    +---+---+---+---+  remains 1
i=2 | 1 |   |   |   |
    +---+---+---+---+
```

#### Filling the DP Table

Now we start filling the table using the formula `dp[i][j] = dp[i][j-1] + dp[i-1][j]`:

For i=1, j=1:
- dp[1][1] = dp[1][0] + dp[0][1] = 1 + 1 = 2

```
    j=0 j=1 j=2 j=3
    +---+---+---+---+
i=0 | 1 | 1 | 1 | 1 |
    +---+---+---+---+
i=1 | 1 | 2 |   |   |  <- dp[1][1] = 1+1 = 2
    +---+---+---+---+
i=2 | 1 |   |   |   |
    +---+---+---+---+
```

For i=1, j=2:
- dp[1][2] = dp[1][1] + dp[0][2] = 2 + 1 = 3

```
    j=0 j=1 j=2 j=3
    +---+---+---+---+
i=0 | 1 | 1 | 1 | 1 |
    +---+---+---+---+
i=1 | 1 | 2 | 3 |   |  <- dp[1][2] = 2+1 = 3
    +---+---+---+---+
i=2 | 1 |   |   |   |
    +---+---+---+---+
```

For i=1, j=3:
- dp[1][3] = dp[1][2] + dp[0][3] = 3 + 1 = 4

```
    j=0 j=1 j=2 j=3
    +---+---+---+---+
i=0 | 1 | 1 | 1 | 1 |
    +---+---+---+---+
i=1 | 1 | 2 | 3 | 4 |  <- dp[1][3] = 3+1 = 4
    +---+---+---+---+
i=2 | 1 |   |   |   |
    +---+---+---+---+
```

For i=2, j=1:
- dp[2][1] = dp[2][0] + dp[1][1] = 1 + 2 = 3

```
    j=0 j=1 j=2 j=3
    +---+---+---+---+
i=0 | 1 | 1 | 1 | 1 |
    +---+---+---+---+
i=1 | 1 | 2 | 3 | 4 |
    +---+---+---+---+
i=2 | 1 | 3 |   |   |  <- dp[2][1] = 1+2 = 3
    +---+---+---+---+
```

For i=2, j=2:
- dp[2][2] = dp[2][1] + dp[1][2] = 3 + 3 = 6

```
    j=0 j=1 j=2 j=3
    +---+---+---+---+
i=0 | 1 | 1 | 1 | 1 |
    +---+---+---+---+
i=1 | 1 | 2 | 3 | 4 |
    +---+---+---+---+
i=2 | 1 | 3 | 6 |   |  <- dp[2][2] = 3+3 = 6
    +---+---+---+---+
```

For i=2, j=3:
- dp[2][3] = dp[2][2] + dp[1][3] = 6 + 4 = 10

```
    j=0 j=1 j=2 j=3
    +---+---+---+---+
i=0 | 1 | 1 | 1 | 1 |
    +---+---+---+---+
i=1 | 1 | 2 | 3 | 4 |
    +---+---+---+---+
i=2 | 1 | 3 | 6 | 10 |  <- dp[2][3] = 6+4 = 10 (Final answer)
    +---+---+---+---+
```

#### JavaScript Implementation

```javascript
/**
 * @param {number} m - Number of rows
 * @param {number} n - Number of columns
 * @return {number} - Number of unique paths
 */
function uniquePaths(m, n) {
    // Create a 2D DP array with dimensions m x n filled with 1s
    const dp = Array(m).fill().map(() => Array(n).fill(1));
    
    // Fill the DP array: each cell contains the sum of the cell above and to the left
    for (let i = 1; i < m; i++) {  // row
        for (let j = 1; j < n; j++) {  // column
            dp[i][j] = dp[i][j-1] + dp[i-1][j];  // left + top
        }
    }
    
    // Return the bottom-right cell which contains the answer
    return dp[m-1][n-1];
}
```

#### Java Implementation

```java
public class Solution {
    public int uniquePaths(int m, int n) {
        // Create a 2D DP array with dimensions m x n filled with 1s
        int[][] dp = new int[m][n];
        
        // Initialize first row and first column with 1s
        for (int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }
        for (int j = 0; j < n; j++) {
            dp[0][j] = 1;
        }
        
        // Fill the DP array: each cell contains the sum of the cell above and to the left
        for (int i = 1; i < m; i++) {  // row
            for (int j = 1; j < n; j++) {  // column
                dp[i][j] = dp[i][j-1] + dp[i-1][j];  // left + top
            }
        }
        
        // Return the bottom-right cell which contains the answer
        return dp[m-1][n-1];
    }
}
```

### 4. Dynamic Programming (Space Optimized)
- **Time complexity**: O(m*n)
- **Space complexity**: O(n)

#### Python
```python
def uniquePaths(m, n):
    row = [1] * n
    
    for i in range(m - 1):
        newRow = [1] * n
        for j in range(n - 2, -1, -1):
            newRow[j] = newRow[j + 1] + row[j]
        row = newRow
    
    return row[0]
```

#### JavaScript
```javascript
function uniquePaths(m, n) {
    let row = Array(n).fill(1);
    
    for (let i = 0; i < m - 1; i++) {
        let newRow = Array(n).fill(1);
        for (let j = n - 2; j >= 0; j--) {
            newRow[j] = newRow[j + 1] + row[j];
        }
        row = newRow;
    }
    
    return row[0];
}
```

#### Java
```java
public int uniquePaths(int m, int n) {
    int[] row = new int[n];
    Arrays.fill(row, 1);
    
    for (int i = 0; i < m - 1; i++) {
        int[] newRow = new int[n];
        newRow[n - 1] = 1;
        
        for (int j = n - 2; j >= 0; j--) {
            newRow[j] = newRow[j + 1] + row[j];
        }
        
        row = newRow;
    }
    
    return row[0];
}
```

### 5. Math (Combinatorial)
- **Time complexity**: O(min(m, n))
- **Space complexity**: O(1)

#### Python
```python
def uniquePaths(m, n):
    if m == 1 or n == 1:
        return 1
    
    if m < n:
        m, n = n, m
    
    res = 1
    j = 1
    
    for i in range(m, m + n - 1):
        res *= i
        res //= j
        j += 1
    
    return res
```

#### JavaScript
```javascript
function uniquePaths(m, n) {
    if (m === 1 || n === 1) {
        return 1;
    }
    
    if (m < n) {
        [m, n] = [n, m];
    }
    
    let res = 1;
    let j = 1;
    
    for (let i = m; i < m + n - 1; i++) {
        res = res * i / j;
        j++;
    }
    
    return Math.round(res);
}
```

#### Java
```java
public int uniquePaths(int m, int n) {
    if (m == 1 || n == 1) {
        return 1;
    }
    
    if (m < n) {
        int temp = m;
        m = n;
        n = temp;
    }
    
    long res = 1;
    int j = 1;
    
    for (int i = m; i < m + n - 1; i++) {
        res *= i;
        res /= j;
        j++;
    }
    
    return (int) res;
}
```
