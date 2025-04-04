# Decode Ways

A message containing letters from A-Z can be encoded into numbers using the following mapping:

'A' -> "1"
'B' -> "2"
...
'Z' -> "26"

To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:

"AAJF" with the grouping (1 1 10 6)
"KJF" with the grouping (11 10 6)

Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".

Given a string s containing only digits, return the number of ways to decode it.

## Approaches

### 1. Recursion

**Time complexity:** O(2^n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        def dfs(i):
            if i == len(s):
                return 1
                
            if s[i] == '0':
                return 0
                
            res = dfs(i + 1)
            
            if i < len(s) - 1:
                if (s[i] == '1' or
                    (s[i] == '2' and s[i + 1] < '7')):
                    res += dfs(i + 2)
                    
            return res
            
        return dfs(0)
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    function dfs(i) {
        if (i === s.length) {
            return 1;
        }
        
        if (s[i] === '0') {
            return 0;
        }
        
        let res = dfs(i + 1);
        
        if (i < s.length - 1) {
            if (s[i] === '1' || (s[i] === '2' && s[i + 1] < '7')) {
                res += dfs(i + 2);
            }
        }
        
        return res;
    }
    
    return dfs(0);
};
```

#### Java
```java
class Solution {
    public int numDecodings(String s) {
        return dfs(s, 0);
    }
    
    private int dfs(String s, int i) {
        if (i == s.length()) {
            return 1;
        }
        
        if (s.charAt(i) == '0') {
            return 0;
        }
        
        int res = dfs(s, i + 1);
        
        if (i < s.length() - 1) {
            if (s.charAt(i) == '1' || 
                (s.charAt(i) == '2' && s.charAt(i + 1) < '7')) {
                res += dfs(s, i + 2);
            }
        }
        
        return res;
    }
}
```

### 2. Dynamic Programming (Top-Down)

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        dp = {len(s) : 1}
        
        def dfs(i):
            if i in dp:
                return dp[i]
                
            if s[i] == "0":
                return 0
                
            res = dfs(i + 1)
            
            if i + 1 < len(s) and (
                s[i] == "1" or s[i] == "2" and
                s[i + 1] in "0123456"
            ):
                res += dfs(i + 2)
                
            dp[i] = res
            return res
            
        return dfs(0)
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    const memo = new Map();
    memo.set(s.length, 1);
    
    function dfs(i) {
        if (memo.has(i)) {
            return memo.get(i);
        }
        
        if (s[i] === '0') {
            return 0;
        }
        
        let res = dfs(i + 1);
        
        if (i + 1 < s.length) {
            if (s[i] === '1' || (s[i] === '2' && s[i + 1] <= '6')) {
                res += dfs(i + 2);
            }
        }
        
        memo.set(i, res);
        return res;
    }
    
    return dfs(0);
};
```

#### Java
```java
class Solution {
    public int numDecodings(String s) {
        Map<Integer, Integer> memo = new HashMap<>();
        memo.put(s.length(), 1);
        
        return dfs(s, 0, memo);
    }
    
    private int dfs(String s, int i, Map<Integer, Integer> memo) {
        if (memo.containsKey(i)) {
            return memo.get(i);
        }
        
        if (s.charAt(i) == '0') {
            return 0;
        }
        
        int res = dfs(s, i + 1, memo);
        
        if (i + 1 < s.length()) {
            if (s.charAt(i) == '1' || 
                (s.charAt(i) == '2' && s.charAt(i + 1) <= '6')) {
                res += dfs(s, i + 2, memo);
            }
        }
        
        memo.put(i, res);
        return res;
    }
}
```

### 3. Dynamic Programming (Bottom-Up)

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        if len(s)==0 or s[0] =='0':
            return 0
        dp = [0]*(len(s)+1)
        dp[0] = 1
        dp[1] = 1
        for i in range(2,len(s)+1):
            if s[i-1]>='1' and s[i-1]<='9':  ##counting for all single char
                dp[i] = dp[i-1]
                       
            if s[i-2]=='1':  ## counting for 10-19 digits
                dp[i] += dp[i-2]
            elif (s[i-2]=='2' and (s[i-1]>='0' and s[i-1]<='6')):
                ## adding for 20 to 26
                dp[i] += dp[i-2]
                   
        return(dp[len(s)])
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    if (s.length === 0 || s[0] === '0') {
        return 0;
    }
    
    const dp = new Array(s.length + 1).fill(0);
    dp[0] = 1;
    dp[1] = 1;
    
    for (let i = 2; i <= s.length; i++) {
        // counting for all single char
        if (s[i-1] >= '1' && s[i-1] <= '9') {
            dp[i] = dp[i-1];
        }
        
        // counting for 10-19 digits
        if (s[i-2] === '1') {
            dp[i] += dp[i-2];
        } 
        // adding for 20 to 26
        else if (s[i-2] === '2' && s[i-1] >= '0' && s[i-1] <= '6') {
            dp[i] += dp[i-2];
        }
    }
    
    return dp[s.length];
};
```

#### Java
```java
class Solution {
    public int numDecodings(String s) {
        if (s.length() == 0 || s.charAt(0) == '0') {
            return 0;
        }
        
        int[] dp = new int[s.length() + 1];
        dp[0] = 1;
        dp[1] = 1;
        
        for (int i = 2; i <= s.length(); i++) {
            // counting for all single char
            if (s.charAt(i-1) >= '1' && s.charAt(i-1) <= '9') {
                dp[i] = dp[i-1];
            }
            
            // counting for 10-19 digits
            if (s.charAt(i-2) == '1') {
                dp[i] += dp[i-2];
            } 
            // adding for 20 to 26
            else if (s.charAt(i-2) == '2' && s.charAt(i-1) >= '0' && s.charAt(i-1) <= '6') {
                dp[i] += dp[i-2];
            }
        }
        
        return dp[s.length()];
    }
}
```

### 4. Dynamic Programming (Space Optimized)

**Time complexity:** O(n)  
**Space complexity:** O(1)

#### Python
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        dp = dp2 = 0
        dp1 = 1
        
        for i in range(len(s) - 1, -1, -1):
            if s[i] == "0":
                dp = 0
            else:
                dp = dp1
                
            if i + 1 < len(s) and (s[i] == "1" or
                                  s[i] == "2" and s[i + 1] in "0123456"
                                 ):
                dp += dp2
                
            dp, dp1, dp2 = 0, dp, dp1
            
        return dp1
```

#### JavaScript
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    let dp1 = 1; // dp[i+1]
    let dp2 = 0; // dp[i+2]
    
    for (let i = s.length - 1; i >= 0; i--) {
        let current = 0;
        
        if (s[i] !== '0') {
            current = dp1;
            
            if (i + 1 < s.length) {
                if (s[i] === '1' || (s[i] === '2' && s[i + 1] <= '6')) {
                    current += dp2;
                }
            }
        }
        
        dp2 = dp1;
        dp1 = current;
    }
    
    return dp1;
};
```

#### Java
```java
class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        int dp1 = 1; // dp[i+1]
        int dp2 = 0; // dp[i+2]
        
        for (int i = n - 1; i >= 0; i--) {
            int current = 0;
            
            if (s.charAt(i) != '0') {
                current = dp1;
                
                if (i + 1 < n) {
                    if (s.charAt(i) == '1' || 
                        (s.charAt(i) == '2' && s.charAt(i + 1) <= '6')) {
                        current += dp2;
                    }
                }
            }
            
            dp2 = dp1;
            dp1 = current;
        }
        
        return dp1;
    }
}
```
