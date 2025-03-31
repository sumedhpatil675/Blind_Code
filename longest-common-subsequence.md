# Longest Common Subsequence

Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, "ace" is a subsequence of "abcde".

A common subsequence of two strings is a subsequence that is common to both strings.

## Approaches

### 1. Recursion
- **Time complexity**: O(2^(m+n))
- **Space complexity**: O(m+n)

#### Python
```python
def longestCommonSubsequence(text1, text2):
    def dfs(i, j):
        if i == len(text1) or j == len(text2):
            return 0
        
        if text1[i] == text2[j]:
            return 1 + dfs(i + 1, j + 1)
        
        return max(dfs(i + 1, j), dfs(i, j + 1))
    
    return dfs(0, 0)
```

#### JavaScript
```javascript
function longestCommonSubsequence(text1, text2) {
    function dfs(i, j) {
        if (i === text1.length || j === text2.length) {
            return 0;
        }
        
        if (text1[i] === text2[j]) {
            return 1 + dfs(i + 1, j + 1);
        }
        
        return Math.max(dfs(i + 1, j), dfs(i, j + 1));
    }
    
    return dfs(0, 0);
}
```

#### Java
```java
public int longestCommonSubsequence(String text1, String text2) {
    return dfs(text1, text2, 0, 0);
}

private int dfs(String text1, String text2, int i, int j) {
    if (i == text1.length() || j == text2.length()) {
        return 0;
    }
    
    if (text1.charAt(i) == text2.charAt(j)) {
        return 1 + dfs(text1, text2, i + 1, j + 1);
    }
    
    return Math.max(dfs(text1, text2, i + 1, j), dfs(text1, text2, i, j + 1));
}
```

### 2. Dynamic Programming (Top-Down)
- **Time complexity**: O(m*n)
- **Space complexity**: O(m*n)

#### Python
```python
def longestCommonSubsequence(text1, text2):
    memo = {}
    
    def dfs(i, j):
        if i == len(text1) or j == len(text2):
            return 0
        
        if (i, j) in memo:
            return memo[(i, j)]
        
        if text1[i] == text2[j]:
            memo[(i, j)] = 1 + dfs(i + 1, j + 1)
        else:
            memo[(i, j)] = max(dfs(i + 1, j), dfs(i, j + 1))
            
        return memo[(i, j)]
    
    return dfs(0, 0)
```

#### JavaScript
```javascript
function longestCommonSubsequence(text1, text2) {
    const memo = new Map();
    
    function dfs(i, j) {
        if (i === text1.length || j === text2.length) {
            return 0;
        }
        
        const key = `${i},${j}`;
        if (memo.has(key)) {
            return memo.get(key);
        }
        
        let result;
        if (text1[i] === text2[j]) {
            result = 1 + dfs(i + 1, j + 1);
        } else {
            result = Math.max(dfs(i + 1, j), dfs(i, j + 1));
        }
        
        memo.set(key, result);
        return result;
    }
    
    return dfs(0, 0);
}
```

#### Java
```java
public int longestCommonSubsequence(String text1, String text2) {
    int[][] memo = new int[text1.length()][text2.length()];
    for (int[] row : memo) {
        Arrays.fill(row, -1);
    }
    return dfs(text1, text2, 0, 0, memo);
}

private int dfs(String text1, String text2, int i, int j, int[][] memo) {
    if (i == text1.length() || j == text2.length()) {
        return 0;
    }
    
    if (memo[i][j] != -1) {
        return memo[i][j];
    }
    
    if (text1.charAt(i) == text2.charAt(j)) {
        memo[i][j] = 1 + dfs(text1, text2, i + 1, j + 1, memo);
    } else {
        memo[i][j] = Math.max(dfs(text1, text2, i + 1, j, memo), dfs(text1, text2, i, j + 1, memo));
    }
    
    return memo[i][j];
}
```

### 3. Dynamic Programming (Bottom-Up)
- **Time complexity**: O(m*n)
- **Space complexity**: O(m*n)

#### Python
```python
def longestCommonSubsequence(text1, text2):
    dp = [[0 for j in range(len(text2) + 1)] for i in range(len(text1) + 1)]
    
    for i in range(len(text1) - 1, -1, -1):
        for j in range(len(text2) - 1, -1, -1):
            if text1[i] == text2[j]:
                dp[i][j] = 1 + dp[i + 1][j + 1]
            else:
                dp[i][j] = max(dp[i][j + 1], dp[i + 1][j])
                
    return dp[0][0]
```

#### JavaScript
```javascript
function longestCommonSubsequence(text1, text2) {
    const dp = Array(text1.length + 1).fill().map(() => Array(text2.length + 1).fill(0));
    
    for (let i = text1.length - 1; i >= 0; i--) {
        for (let j = text2.length - 1; j >= 0; j--) {
            if (text1[i] === text2[j]) {
                dp[i][j] = 1 + dp[i + 1][j + 1];
            } else {
                dp[i][j] = Math.max(dp[i][j + 1], dp[i + 1][j]);
            }
        }
    }
    
    return dp[0][0];
}
```

#### Java
```java
public int longestCommonSubsequence(String text1, String text2) {
    int[][] dp = new int[text1.length() + 1][text2.length() + 1];
    
    for (int i = text1.length() - 1; i >= 0; i--) {
        for (int j = text2.length() - 1; j >= 0; j--) {
            if (text1.charAt(i) == text2.charAt(j)) {
                dp[i][j] = 1 + dp[i + 1][j + 1];
            } else {
                dp[i][j] = Math.max(dp[i][j + 1], dp[i + 1][j]);
            }
        }
    }
    
    return dp[0][0];
}
```

### 4. Dynamic Programming (Space Optimized)
- **Time complexity**: O(m*n)
- **Space complexity**: O(min(m,n))

#### Python
```python
def longestCommonSubsequence(text1, text2):
    if len(text1) < len(text2):
        text1, text2 = text2, text1
    
    prev = [0] * (len(text2) + 1)
    curr = [0] * (len(text2) + 1)
    
    for i in range(len(text1) - 1, -1, -1):
        for j in range(len(text2) - 1, -1, -1):
            if text1[i] == text2[j]:
                curr[j] = 1 + prev[j + 1]
            else:
                curr[j] = max(curr[j + 1], prev[j])
        
        prev, curr = curr, prev
    
    return prev[0]
```

#### JavaScript
```javascript
function longestCommonSubsequence(text1, text2) {
    if (text1.length < text2.length) {
        [text1, text2] = [text2, text1];
    }
    
    let prev = Array(text2.length + 1).fill(0);
    let curr = Array(text2.length + 1).fill(0);
    
    for (let i = text1.length - 1; i >= 0; i--) {
        for (let j = text2.length - 1; j >= 0; j--) {
            if (text1[i] === text2[j]) {
                curr[j] = 1 + prev[j + 1];
            } else {
                curr[j] = Math.max(curr[j + 1], prev[j]);
            }
        }
        
        [prev, curr] = [curr, prev];
    }
    
    return prev[0];
}
```

#### Java
```java
public int longestCommonSubsequence(String text1, String text2) {
    if (text1.length() < text2.length()) {
        String temp = text1;
        text1 = text2;
        text2 = temp;
    }
    
    int[] prev = new int[text2.length() + 1];
    int[] curr = new int[text2.length() + 1];
    
    for (int i = text1.length() - 1; i >= 0; i--) {
        for (int j = text2.length() - 1; j >= 0; j--) {
            if (text1.charAt(i) == text2.charAt(j)) {
                curr[j] = 1 + prev[j + 1];
            } else {
                curr[j] = Math.max(curr[j + 1], prev[j]);
            }
        }
        
        int[] temp = prev;
        prev = curr;
        curr = temp;
    }
    
    return prev[0];
}
```

### 5. Dynamic Programming (Optimal)
- **Time complexity**: O(m*n)
- **Space complexity**: O(min(m,n))

#### Python
```python
def longestCommonSubsequence(text1, text2):
    if len(text1) < len(text2):
        text1, text2 = text2, text1
    
    dp = [0] * (len(text2) + 1)
    
    for i in range(len(text1) - 1, -1, -1):
        prev = 0
        for j in range(len(text2) - 1, -1, -1):
            temp = dp[j]
            if text1[i] == text2[j]:
                dp[j] = 1 + prev
            else:
                dp[j] = max(dp[j], dp[j + 1])
            prev = temp
    
    return dp[0]
```

#### JavaScript
```javascript
function longestCommonSubsequence(text1, text2) {
    if (text1.length < text2.length) {
        [text1, text2] = [text2, text1];
    }
    
    let dp = Array(text2.length + 1).fill(0);
    
    for (let i = text1.length - 1; i >= 0; i--) {
        let prev = 0;
        for (let j = text2.length - 1; j >= 0; j--) {
            let temp = dp[j];
            if (text1[i] === text2[j]) {
                dp[j] = 1 + prev;
            } else {
                dp[j] = Math.max(dp[j], dp[j + 1]);
            }
            prev = temp;
        }
    }
    
    return dp[0];
}
```

#### Java
```java
public int longestCommonSubsequence(String text1, String text2) {
    if (text1.length() < text2.length()) {
        String temp = text1;
        text1 = text2;
        text2 = temp;
    }
    
    int[] dp = new int[text2.length() + 1];
    
    for (int i = text1.length() - 1; i >= 0; i--) {
        int prev = 0;
        for (int j = text2.length() - 1; j >= 0; j--) {
            int temp = dp[j];
            if (text1.charAt(i) == text2.charAt(j)) {
                dp[j] = 1 + prev;
            } else {
                dp[j] = Math.max(dp[j], dp[j + 1]);
            }
            prev = temp;
        }
    }
    
    return dp[0];
}
```