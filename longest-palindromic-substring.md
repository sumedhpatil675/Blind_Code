# Longest Palindromic Substring

Given a string s, return the longest palindromic substring in s.

## Approaches

### 1. Brute Force

**Time complexity:** O(n³)  
**Space complexity:** O(1)

#### Python
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        res, resLen = "", 0
        
        for i in range(len(s)):
            for j in range(i, len(s)):
                l, r = i, j
                
                while l < r and s[l] == s[r]:
                    l += 1
                    r -= 1
                    
                if l >= r and resLen < (j - i + 1):
                    res = s[i : j + 1]
                    resLen = j - i + 1
                    
        return res
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    let result = "";
    let maxLength = 0;
    
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
            
            if (isPalindrome && (j - i + 1) > maxLength) {
                result = s.substring(i, j + 1);
                maxLength = j - i + 1;
            }
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public String longestPalindrome(String s) {
        String result = "";
        int maxLength = 0;
        
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
                
                if (isPalindrome && (j - i + 1) > maxLength) {
                    result = s.substring(i, j + 1);
                    maxLength = j - i + 1;
                }
            }
        }
        
        return result;
    }
}
```

### 2. Dynamic Programming

**Time complexity:** O(n²)  
**Space complexity:** O(n²)

#### Python
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        resIdx, resLen = 0, 0
        n = len(s)
        
        dp = [[False] * n for _ in range(n)]
        
        for i in range(n - 1, -1, -1):
            for j in range(i, n):
                if s[i] == s[j] and (j - i <= 2 or dp[i + 1][j - 1]):
                    dp[i][j] = True
                    
                    if resLen < (j - i + 1):
                        resIdx = i
                        resLen = j - i + 1
                        
        return s[resIdx : resIdx + resLen]
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    if (!s || s.length < 1) return "";
    
    let start = 0;
    let maxLength = 1;
    const n = s.length;
    
    // Initialize DP table
    const dp = Array(n).fill().map(() => Array(n).fill(false));
    
    // All single characters are palindromes
    for (let i = 0; i < n; i++) {
        dp[i][i] = true;
    }
    
    // Check for palindromes of length 2
    for (let i = 0; i < n - 1; i++) {
        if (s[i] === s[i + 1]) {
            dp[i][i + 1] = true;
            start = i;
            maxLength = 2;
        }
    }
    
    // Check for palindromes of length > 2
    for (let len = 3; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const j = i + len - 1;
            
            if (dp[i + 1][j - 1] && s[i] === s[j]) {
                dp[i][j] = true;
                start = i;
                maxLength = len;
            }
        }
    }
    
    return s.substring(start, start + maxLength);
};
```

#### Java
```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) return "";
        
        int start = 0;
        int maxLength = 1;
        int n = s.length();
        
        // Initialize DP table
        boolean[][] dp = new boolean[n][n];
        
        // All single characters are palindromes
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
        }
        
        // Check for palindromes of length 2
        for (int i = 0; i < n - 1; i++) {
            if (s.charAt(i) == s.charAt(i + 1)) {
                dp[i][i + 1] = true;
                start = i;
                maxLength = 2;
            }
        }
        
        // Check for palindromes of length > 2
        for (int len = 3; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1;
                
                if (dp[i + 1][j - 1] && s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = true;
                    start = i;
                    maxLength = len;
                }
            }
        }
        
        return s.substring(start, start + maxLength);
    }
}
```

### 3. Two Pointers

**Time complexity:** O(n²)  
**Space complexity:** O(1)

#### Python
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        resIdx = 0
        resLen = 0
        
        for i in range(len(s)):
            # odd length
            l, r = i, i
            while l >= 0 and r < len(s) and s[l] == s[r]:
                if (r - l + 1) > resLen:
                    resIdx = l
                    resLen = r - l + 1
                l -= 1
                r += 1
                
            # even length
            l, r = i, i + 1
            while l >= 0 and r < len(s) and s[l] == s[r]:
                if (r - l + 1) > resLen:
                    resIdx = l
                    resLen = r - l + 1
                l -= 1
                r += 1
                
        return s[resIdx : resIdx + resLen]
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    let start = 0;
    let maxLength = 1;
    
    function expandAroundCenter(left, right) {
        while (left >= 0 && right < s.length && s[left] === s[right]) {
            const currentLength = right - left + 1;
            if (currentLength > maxLength) {
                maxLength = currentLength;
                start = left;
            }
            left--;
            right++;
        }
    }
    
    for (let i = 0; i < s.length; i++) {
        // Odd length palindromes
        expandAroundCenter(i, i);
        // Even length palindromes
        expandAroundCenter(i, i + 1);
    }
    
    return s.substring(start, start + maxLength);
};
```

#### Java
```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) return "";
        
        int start = 0;
        int maxLength = 1;
        
        for (int i = 0; i < s.length(); i++) {
            // Odd length palindromes
            int len1 = expandAroundCenter(s, i, i);
            // Even length palindromes
            int len2 = expandAroundCenter(s, i, i + 1);
            int len = Math.max(len1, len2);
            
            if (len > maxLength) {
                maxLength = len;
                start = i - (len - 1) / 2;
            }
        }
        
        return s.substring(start, start + maxLength);
    }
    
    private int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        
        return right - left - 1;
    }
}
```

### 4. Manacher's Algorithm

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
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
        resLen, center_idx = max((v, i) for i, v in enumerate(p))
        resIdx = (center_idx - resLen) // 2
        
        return s[resIdx : resIdx + resLen]
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
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
            let a = i + (1 + p[i]);
            let b = i - (1 + p[i]);
            while (a < n && b >= 0 && t[a] === t[b]) {
                p[i]++;
                a++;
                b--;
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
    let maxLen = 0;
    let centerIndex = 0;
    
    for (let i = 0; i < p.length; i++) {
        if (p[i] > maxLen) {
            maxLen = p[i];
            centerIndex = i;
        }
    }
    
    // Calculate original string indices
    const start = Math.floor((centerIndex - maxLen) / 2);
    return s.substring(start, start + maxLen);
};
```

#### Java
```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) return "";
        
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
        
        // Find the maximum element in p
        int maxLen = 0;
        int centerIndex = 0;
        for (int i = 0; i < n; i++) {
            if (p[i] > maxLen) {
                maxLen = p[i];
                centerIndex = i;
            }
        }
        
        // Extract the palindrome
        int start = (centerIndex - maxLen) / 2;
        return s.substring(start, start + maxLen);
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