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

# Longest Common Subsequence DP Table

**Example**: text1 = "ABCDE", text2 = "ACE"

```
      |     |  A  |  C  |  E  |
------|-----|-----|-----|-----|
      |  0  |  0  |  0  |  0  |
------|-----|-----|-----|-----|
  A   |  0  |  1* |  1  |  1  |
------|-----|-----|-----|-----|
  B   |  0  |  1  |  1  |  1  |
------|-----|-----|-----|-----|
  C   |  0  |  1  |  2* |  2  |
------|-----|-----|-----|-----|
  D   |  0  |  1  |  2  |  2  |
------|-----|-----|-----|-----|
  E   |  0  |  1  |  2  |  3* |
------|-----|-----|-----|-----|
```

*\* Indicates a character match (diagonal + 1)*

**Result**: LCS length = 3 (The sequence is "ACE")
- 

## Python
```python
def longestCommonSubsequence(text1, text2):
    # m = length of first string
    m = len(text1)
    # n = length of second string
    n = len(text2)
    
    # Create a 2D array of size (m+1)x(n+1) filled with zeros
    # res[i][j] will store the length of LCS of text1[0...i-1] and text2[0...j-1]
    res = [[0]*(n+1)for _ in range(m+1)]
    
    # Iterate through each character of text1
    for i in range(1,m+1): ## i represents position in text1 (1-indexed for dp array)
        # Iterate through each character of text2
        for j in range(1,n+1): ## j represents position in text2 (1-indexed for dp array)
            # If current characters match
            # text1[i-1] is the current character in text1 (0-indexed in the string)
            # text2[j-1] is the current character in text2 (0-indexed in the string)
            if text1[i-1]==text2[j-1]: 
                # Add 1 to the LCS of the strings without these characters
                # res[i-1][j-1] = LCS without the current character from both strings
                res[i][j] = 1+res[i-1][j-1]
            else: 
                # Characters don't match, take the maximum from:
                # res[i][j-1] = LCS when we exclude current character from text2
                # res[i-1][j] = LCS when we exclude current character from text1
                res[i][j] = max(res[i][j-1],res[i-1][j])
    
    # Return the length of LCS for the entire strings
    return(res[m][n])
```

## JavaScript
```javascript
/**
 * @param {string} text1 - First input string
 * @param {string} text2 - Second input string
 * @return {number} - Length of longest common subsequence
 */
function longestCommonSubsequence(text1, text2) {
    // m = length of first string
    const m = text1.length;
    // n = length of second string
    const n = text2.length;
    
    // Create a 2D array of size (m+1)x(n+1) filled with zeros
    // res[i][j] will store the length of LCS of text1[0...i-1] and text2[0...j-1]
    const res = Array(m+1).fill().map(() => Array(n+1).fill(0));
    
    // Iterate through each character of text1
    for (let i = 1; i <= m; i++) { // i represents position in text1 (1-indexed for dp array)
        // Iterate through each character of text2
        for (let j = 1; j <= n; j++) { // j represents position in text2 (1-indexed for dp array)
            // If current characters match
            // text1[i-1] is the current character in text1 (0-indexed in the string)
            // text2[j-1] is the current character in text2 (0-indexed in the string)
            if (text1[i-1] === text2[j-1]) {
                // Add 1 to the LCS of the strings without these characters
                // res[i-1][j-1] = LCS without the current character from both strings
                res[i][j] = 1 + res[i-1][j-1];
            } else {
                // Characters don't match, take the maximum from:
                // res[i][j-1] = LCS when we exclude current character from text2
                // res[i-1][j] = LCS when we exclude current character from text1
                res[i][j] = Math.max(res[i][j-1], res[i-1][j]);
            }
        }
    }
    
    // Return the length of LCS for the entire strings
    return res[m][n];
}
```

## Java
```java
class Solution {
    /**
     * Find the length of the Longest Common Subsequence of two strings
     * 
     * @param text1 First input string
     * @param text2 Second input string
     * @return Length of longest common subsequence
     */
    public int longestCommonSubsequence(String text1, String text2) {
        // m = length of first string
        int m = text1.length();
        // n = length of second string
        int n = text2.length();
        
        // Create a 2D array of size (m+1)x(n+1) filled with zeros
        // res[i][j] will store the length of LCS of text1[0...i-1] and text2[0...j-1]
        int[][] res = new int[m+1][n+1];
        
        // Iterate through each character of text1
        for (int i = 1; i <= m; i++) { // i represents position in text1 (1-indexed for dp array)
            // Iterate through each character of text2
            for (int j = 1; j <= n; j++) { // j represents position in text2 (1-indexed for dp array)
                // If current characters match
                // text1.charAt(i-1) is the current character in text1 (0-indexed in the string)
                // text2.charAt(j-1) is the current character in text2 (0-indexed in the string)
                if (text1.charAt(i-1) == text2.charAt(j-1)) {
                    // Add 1 to the LCS of the strings without these characters
                    // res[i-1][j-1] = LCS without the current character from both strings
                    res[i][j] = 1 + res[i-1][j-1];
                } else {
                    // Characters don't match, take the maximum from:
                    // res[i][j-1] = LCS when we exclude current character from text2
                    // res[i-1][j] = LCS when we exclude current character from text1
                    res[i][j] = Math.max(res[i][j-1], res[i-1][j]);
                }
            }
        }
        
        // Return the length of LCS for the entire strings
        return res[m][n];
    }
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
