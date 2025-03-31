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

### 3. Dynamic Programming (Bottom-Up)
- **Time complexity**: O(m*n)
- **Space complexity**: O(m*n)

#### Python
```python
def uniquePaths(m, n):
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    dp[m - 1][n - 1] = 1
    
    for i in range(m - 1, -1, -1):
        for j in range(n - 1, -1, -1):
            dp[i][j] += dp[i + 1][j] + dp[i][j + 1]
    
    return dp[0][0]
```

#### JavaScript
```javascript
function uniquePaths(m, n) {
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    dp[m - 1][n - 1] = 1;
    
    for (let i = m - 1; i >= 0; i--) {
        for (let j = n - 1; j >= 0; j--) {
            if (i < m - 1 || j < n - 1) { // Skip the destination cell
                dp[i][j] = dp[i + 1][j] + dp[i][j + 1];
            }
        }
    }
    
    return dp[0][0];
}
```

#### Java
```java
public int uniquePaths(int m, int n) {
    int[][] dp = new int[m + 1][n + 1];
    dp[m - 1][n - 1] = 1;
    
    for (int i = m - 1; i >= 0; i--) {
        for (int j = n - 1; j >= 0; j--) {
            if (i < m - 1 || j < n - 1) { // Skip the destination cell
                dp[i][j] += dp[i + 1][j] + dp[i][j + 1];
            }
        }
    }
    
    return dp[0][0];
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