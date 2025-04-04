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
Let's visualize the DP array for the input string `"12345"` using ASCII characters, with correct handling of valid digit combinations.

## Input: "12345"

```
Index:  0  1  2  3  4  5  
String:    1  2  3  4  5  
DP:     1  1  ?  ?  ?  ?  
```

Initial setup:
- dp[0] = 1 (base case)
- dp[1] = 1 (since s[0] = '1' which is a valid single digit)

Let's fill the dp array step by step:

### Step 1: Fill dp[2]

For dp[2], we need to check:
1. Is s[1] a valid single digit? Yes, s[1] = '2', so dp[2] += dp[1] = 1
2. Is s[0]s[1] = "12" a valid double digit? Yes, "12" is valid (10-26), so dp[2] += dp[0] = 1

```
Index:  0  1  2  3  4  5  
String:    1  2  3  4  5  
DP:     1  1  2  ?  ?  ?  
         ↑  ↑  
         |  |  
         |  +-- Valid single digit: '2'
         +---- Valid double digit: "12"
```

### Step 2: Fill dp[3]

For dp[3], we need to check:
1. Is s[2] a valid single digit? Yes, s[2] = '3', so dp[3] += dp[2] = 2
2. Is s[1]s[2] = "23" a valid double digit? Yes, "23" is valid (10-26), so dp[3] += dp[1] = 1

```
Index:  0  1  2  3  4  5  
String:    1  2  3  4  5  
DP:     1  1  2  3  ?  ?  
            ↑  ↑  
            |  |  
            |  +-- Valid single digit: '3'
            +---- Valid double digit: "23"
```

### Step 3: Fill dp[4]

For dp[4], we need to check:
1. Is s[3] a valid single digit? Yes, s[3] = '4', so dp[4] += dp[3] = 3
2. Is s[2]s[3] = "34" a valid double digit? No, "34" is NOT valid (>26)

```
Index:  0  1  2  3  4  5  
String:    1  2  3  4  5  
DP:     1  1  2  3  3  ?  
               ↑  
               |  
               +-- Only valid as single digit: '4'
```

### Step 4: Fill dp[5]

For dp[5], we need to check:
1. Is s[4] a valid single digit? Yes, s[4] = '5', so dp[5] += dp[4] = 3
2. Is s[3]s[4] = "45" a valid double digit? No, "45" is NOT valid (>26)

```
Index:  0  1  2  3  4  5  
String:    1  2  3  4  5  
DP:     1  1  2  3  3  3  
                  ↑  
                  |  
                  +-- Only valid as single digit: '5'
```

Therefore, there are 3 ways to decode the string "12345".

## Let's try another example: "226"

```
Index:  0  1  2  3  
String:    2  2  6  
DP:     1  1  ?  ?  
```

Initial setup:
- dp[0] = 1 (base case)
- dp[1] = 1 (since s[0] = '2' which is a valid single digit)

### Step 1: Fill dp[2]

For dp[2], we need to check:
1. Is s[1] a valid single digit? Yes, s[1] = '2', so dp[2] += dp[1] = 1
2. Is s[0]s[1] = "22" a valid double digit? Yes, "22" is valid (10-26), so dp[2] += dp[0] = 1

```
Index:  0  1  2  3  
String:    2  2  6  
DP:     1  1  2  ?  
         ↑  ↑  
         |  |  
         |  +-- Valid single digit: '2'
         +---- Valid double digit: "22"
```

### Step 2: Fill dp[3]

For dp[3], we need to check:
1. Is s[2] a valid single digit? Yes, s[2] = '6', so dp[3] += dp[2] = 2
2. Is s[1]s[2] = "26" a valid double digit? Yes, "26" is valid (=26), so dp[3] += dp[1] = 1

```
Index:  0  1  2  3  
String:    2  2  6  
DP:     1  1  2  3  
            ↑  ↑  
            |  |  
            |  +-- Valid single digit: '6'
            +---- Valid double digit: "26"
```

Therefore, there are 3 ways to decode the string "226".

## Example with a zero: "1201"

```
Index:  0  1  2  3  4  
String:    1  2  0  1  
DP:     1  1  ?  ?  ?  
```

### Step 1: Fill dp[2]

For dp[2], we need to check:
1. Is s[1] a valid single digit? Yes, s[1] = '2', so dp[2] += dp[1] = 1
2. Is s[0]s[1] = "12" a valid double digit? Yes, "12" is valid (10-26), so dp[2] += dp[0] = 1

```
Index:  0  1  2  3  4  
String:    1  2  0  1  
DP:     1  1  2  ?  ?  
```

### Step 2: Fill dp[3]

For dp[3], we need to check:
1. Is s[2] a valid single digit? No, s[2] = '0', so no addition here
2. Is s[1]s[2] = "20" a valid double digit? Yes, "20" is valid (10-26), so dp[3] += dp[1] = 1

```
Index:  0  1  2  3  4  
String:    1  2  0  1  
DP:     1  1  2  1  ?  
            ↑     
            |     
            +---- Only valid as double digit: "20"
```

### Step 3: Fill dp[4]

For dp[4], we need to check:
1. Is s[3] a valid single digit? Yes, s[3] = '1', so dp[4] += dp[3] = 1
2. Is s[2]s[3] = "01" a valid double digit? No, "01" is not valid due to leading zero

```
Index:  0  1  2  3  4  
String:    1  2  0  1  
DP:     1  1  2  1  1  
               ↑     
               |     
               +---- Valid single digit: '1'
```

Therefore, there is only 1 way to decode the string "1201".

## One more example: "121"

```
Index:  0  1  2  3  
String:    1  2  1  
DP:     1  1  ?  ?  
```

### Step 1: Fill dp[2]

For dp[2], we need to check:
1. Is s[1] a valid single digit? Yes, s[1] = '2', so dp[2] += dp[1] = 1
2. Is s[0]s[1] = "12" a valid double digit? Yes, "12" is valid (10-26), so dp[2] += dp[0] = 1

```
Index:  0  1  2  3  
String:    1  2  1  
DP:     1  1  2  ?  
```

### Step 2: Fill dp[3]

For dp[3], we need to check:
1. Is s[2] a valid single digit? Yes, s[2] = '1', so dp[3] += dp[2] = 2
2. Is s[1]s[2] = "21" a valid double digit? Yes, "21" is valid (10-26), so dp[3] += dp[1] = 1

```
Index:  0  1  2  3  
String:    1  2  1  
DP:     1  1  2  3  
            ↑  ↑  
            |  |  
            |  +-- Valid single digit: '1'
            +---- Valid double digit: "21"
```

Therefore, there are 3 ways to decode the string "121".


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
