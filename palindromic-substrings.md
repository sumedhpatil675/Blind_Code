# Palindromic Substrings

Given a string s, return the number of palindromic substrings in it.

A string is a palindrome when it reads the same backward as forward.

A substring is a contiguous sequence of characters within the string.

## Approaches

### 1. Brute Force

**Time complexity:** O(n³)  
**Space complexity:** O(1)

#### Python
```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        res = 0
        
        for i in range(len(s)):
            for j in range(i, len(s)):
                l, r = i, j
                
                while l < r and s[l] == s[r]:
                    l += 1
                    r -= 1
                    
                res += (l >= r)
                
        return res
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var countSubstrings = function(s) {
    let count = 0;
    
    for (let i = 0; i < s.length; i++) {
        for (let j = i; j < s.length; j++) {
            let left = i;
            let right = j;
            let isPalindrome = true;
            
            while (left < right) {
                if (s[left] !== s[right]) {
                    isPalindrome = false;
                    break;
                }
                left++;
                right--;
            }
            
            if (isPalindrome) {
                count++;
            }
        }
    }
    
    return count;
};
```

#### Java
```java
class Solution {
    public int countSubstrings(String s) {
        int count = 0;
        
        for (int i = 0; i < s.length(); i++) {
            for (int j = i; j < s.length(); j++) {
                int left = i;
                int right = j;
                boolean isPalindrome = true;
                
                while (left < right) {
                    if (s.charAt(left) != s.charAt(right)) {
                        isPalindrome = false;
                        break;
                    }
                    left++;
                    right--;
                }
                
                if (isPalindrome) {
                    count++;
                }
            }
        }
        
        return count;
    }
}
```

### 2. Dynamic Programming

**Time complexity:** O(n²)  
**Space complexity:** O(n²)

#### Python
```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        n, res = len(s), 0
        
        dp = [[False] * n for _ in range(n)]
        
        for i in range(n - 1, -1, -1):
            for j in range(i, n):
                if s[i] == s[j] and (j - i <= 2 or dp[i + 1][j - 1]):
                    dp[i][j] = True
                    res += 1
                    
        return res
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var countSubstrings = function(s) {
    const n = s.length;
    let count = 0;
    
    // Initialize DP table
    const dp = Array(n).fill().map(() => Array(n).fill(false));
    
    // Fill the table diagonally
    for (let i = n - 1; i >= 0; i--) {
        for (let j = i; j < n; j++) {
            // Cases:
            // 1. Single character (i == j)
            // 2. Two characters (j - i == 1) and they match
            // 3. More characters and the first and last match, and the substring between is a palindrome
            if (s[i] === s[j] && (j - i <= 2 || dp[i + 1][j - 1])) {
                dp[i][j] = true;
                count++;
            }
        }
    }
    
    return count;
};
```

#### Java
```java
class Solution {
    public int countSubstrings(String s) {
        int n = s.length();
        int count = 0;
        
        // Initialize DP table
        boolean[][] dp = new boolean[n][n];
        
        // Fill the table diagonally
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                // Cases:
                // 1. Single character (i == j)
                // 2. Two characters (j - i == 1) and they match
                // 3. More characters and the first and last match, and the substring between is a palindrome
                if (s.charAt(i) == s.charAt(j) && (j - i <= 2 || dp[i + 1][j - 1])) {
                    dp[i][j] = true;
                    count++;
                }
            }
        }
        
        return count;
    }
}
```

### 3. Two Pointers ✅✅✅

**Time complexity:** O(n²)  
**Space complexity:** O(1)

#### Python
```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        res = 0
        
        for i in range(len(s)):
            # odd length
            l, r = i, i
            while l >= 0 and r < len(s) and s[l] == s[r]:
                res += 1
                l -= 1
                r += 1
                
            # even length
            l, r = i, i + 1
            while l >= 0 and r < len(s) and s[l] == s[r]:
                res += 1
                l -= 1
                r += 1
                
        return res
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var countSubstrings = function(s) {
    let count = 0;
    
    for (let i = 0; i < s.length; i++) {
        // Odd length palindromes
        expandAroundCenter(i, i);
        // Even length palindromes
        expandAroundCenter(i, i + 1);
    }
    
    function expandAroundCenter(left, right) {
        while (left >= 0 && right < s.length && s[left] === s[right]) {
            count++;
            left--;
            right++;
        }
    }
    
    return count;
};
```

#### Java
```java
class Solution {
    public int countSubstrings(String s) {
        int count = 0;
        
        for (int i = 0; i < s.length(); i++) {
            // Odd length palindromes
            count += expandAroundCenter(s, i, i);
            // Even length palindromes
            count += expandAroundCenter(s, i, i + 1);
        }
        
        return count;
    }
    
    private int expandAroundCenter(String s, int left, int right) {
        int count = 0;
        
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            count++;
            left--;
            right++;
        }
        
        return count;
    }
}
```

### 4. Two Pointers (Optimal)

**Time complexity:** O(n²)  
**Space complexity:** O(1)

#### Python
```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        res = 0
        
        for i in range(len(s)):
            res += self.countPali(s, i, i)
            res += self.countPali(s, i, i + 1)
            
        return res
    
    def countPali(self, s, l, r):
        res = 0
        
        while l >= 0 and r < len(s) and s[l] == s[r]:
            res += 1
            l -= 1
            r += 1
            
        return res
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var countSubstrings = function(s) {
    let count = 0;
    
    function countPalindromes(left, right) {
        let count = 0;
        
        while (left >= 0 && right < s.length && s[left] === s[right]) {
            count++;
            left--;
            right++;
        }
        
        return count;
    }
    
    for (let i = 0; i < s.length; i++) {
        // Count odd length palindromes
        count += countPalindromes(i, i);
        // Count even length palindromes
        count += countPalindromes(i, i + 1);
    }
    
    return count;
};
```

#### Java
```java
class Solution {
    public int countSubstrings(String s) {
        int count = 0;
        
        for (int i = 0; i < s.length(); i++) {
            // Count odd length palindromes
            count += countPalindromes(s, i, i);
            // Count even length palindromes
            count += countPalindromes(s, i, i + 1);
        }
        
        return count;
    }
    
    private int countPalindromes(String s, int left, int right) {
        int count = 0;
        
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            count++;
            left--;
            right++;
        }
        
        return count;
    }
}
```

### 5. Manacher's Algorithm

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        def manacher(s):
            t = '#' + '#'.join(s) + '#'
            n = len(t)
            p = [0] * n
            l, r = 0, 0
            
            for i in range(n):
                p[i] = min(r - i, p[l + (r - i)]) if i < r else 0
                
                while (i + p[i] + 1 < n and i - p[i] - 1 >= 0
                      and t[i + p[i] + 1] == t[i - p[i] - 1]):
                    p[i] += 1
                    
                if i + p[i] > r:
                    l, r = i - p[i], i + p[i]
                    
            return p
        
        p = manacher(s)
        res = 0
        
        for i in p:
            res += (i + 1) // 2
            
        return res
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var countSubstrings = function(s) {
    function manacher(s) {
        // Transform string by inserting '#' between each character
        const t = '#' + s.split('').join('#') + '#';
        const n = t.length;
        const p = Array(n).fill(0);
        
        let center = 0;
        let right = 0;
        
        for (let i = 0; i < n; i++) {
            if (i < right) {
                p[i] = Math.min(right - i, p[2 * center - i]);
            }
            
            // Expand around center i
            while (i + 1 + p[i] < n && i - 1 - p[i] >= 0 && t[i + 1 + p[i]] === t[i - 1 - p[i]]) {
                p[i]++;
            }
            
            // Update center and right boundary if needed
            if (i + p[i] > right) {
                center = i;
                right = i + p[i];
            }
        }
        
        return p;
    }
    
    const p = manacher(s);
    
    // Count palindromes
    return p.reduce((count, radius) => count + Math.floor((radius + 1) / 2), 0);
};
```

#### Java
```java
class Solution {
    public int countSubstrings(String s) {
        // Preprocess the string
        String t = preprocess(s);
        int n = t.length();
        int[] p = new int[n];
        
        int center = 0, right = 0;
        
        for (int i = 0; i < n; i++) {
            if (i < right) {
                p[i] = Math.min(right - i, p[2 * center - i]);
            }
            
            // Expand around center i
            while (i + 1 + p[i] < n && i - 1 - p[i] >= 0 && 
                  t.charAt(i + 1 + p[i]) == t.charAt(i - 1 - p[i])) {
                p[i]++;
            }
            
            // Update center and right boundary if needed
            if (i + p[i] > right) {
                center = i;
                right = i + p[i];
            }
        }
        
        // Count palindromes
        int count = 0;
        for (int radius : p) {
            count += (radius + 1) / 2;
        }
        
        return count;
    }
    
    private String preprocess(String s) {
        StringBuilder sb = new StringBuilder();
        sb.append('^');
        for (char c : s.toCharArray()) {
            sb.append('#').append(c);
        }
        sb.append('#').append('$');
        return sb.toString();
    }
}
```
