# Jump Game

You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.

## Approaches

### 1. Recursion
- **Time complexity**: O(n!)
- **Space complexity**: O(n)

#### Python
```python
def canJump(nums):
    def dfs(i):
        if i == len(nums) - 1:
            return True
        
        end = min(len(nums) - 1, i + nums[i])
        for j in range(i + 1, end + 1):
            if dfs(j):
                return True
        
        return False
    
    return dfs(0)
```

#### JavaScript
```javascript
function canJump(nums) {
    function dfs(i) {
        if (i === nums.length - 1) {
            return true;
        }
        
        const end = Math.min(nums.length - 1, i + nums[i]);
        for (let j = i + 1; j <= end; j++) {
            if (dfs(j)) {
                return true;
            }
        }
        
        return false;
    }
    
    return dfs(0);
}
```

#### Java
```java
public boolean canJump(int[] nums) {
    return dfs(nums, 0);
}

private boolean dfs(int[] nums, int i) {
    if (i == nums.length - 1) {
        return true;
    }
    
    int end = Math.min(nums.length - 1, i + nums[i]);
    for (int j = i + 1; j <= end; j++) {
        if (dfs(nums, j)) {
            return true;
        }
    }
    
    return false;
}
```

### 2. Dynamic Programming (Top-Down)
- **Time complexity**: O(n²)
- **Space complexity**: O(n)

#### Python
```python
def canJump(nums):
    memo = {}
    
    def dfs(i):
        if i in memo:
            return memo[i]
        
        if i == len(nums) - 1:
            return True
        
        if nums[i] == 0:
            return False
        
        end = min(len(nums), i + nums[i] + 1)
        for j in range(i + 1, end):
            if dfs(j):
                memo[i] = True
                return True
        
        memo[i] = False
        return False
    
    return dfs(0)
```

#### JavaScript
```javascript
function canJump(nums) {
    const memo = new Map();
    
    function dfs(i) {
        if (memo.has(i)) {
            return memo.get(i);
        }
        
        if (i === nums.length - 1) {
            return true;
        }
        
        if (nums[i] === 0) {
            return false;
        }
        
        const end = Math.min(nums.length, i + nums[i] + 1);
        for (let j = i + 1; j < end; j++) {
            if (dfs(j)) {
                memo.set(i, true);
                return true;
            }
        }
        
        memo.set(i, false);
        return false;
    }
    
    return dfs(0);
}
```

#### Java
```java
public boolean canJump(int[] nums) {
    Boolean[] memo = new Boolean[nums.length];
    return dfs(nums, 0, memo);
}

private boolean dfs(int[] nums, int i, Boolean[] memo) {
    if (memo[i] != null) {
        return memo[i];
    }
    
    if (i == nums.length - 1) {
        return true;
    }
    
    if (nums[i] == 0) {
        return false;
    }
    
    int end = Math.min(nums.length, i + nums[i] + 1);
    for (int j = i + 1; j < end; j++) {
        if (dfs(nums, j, memo)) {
            memo[i] = true;
            return true;
        }
    }
    
    memo[i] = false;
    return false;
}
```

### 3. Dynamic Programming (Bottom-Up)
- **Time complexity**: O(n²)
- **Space complexity**: O(n)

#### Python
```python
def canJump(nums):
    n = len(nums)
    dp = [False] * n
    dp[-1] = True
    
    for i in range(n - 2, -1, -1):
        end = min(n, i + nums[i] + 1)
        for j in range(i + 1, end):
            if dp[j]:
                dp[i] = True
                break
    
    return dp[0]
```

#### JavaScript
```javascript
function canJump(nums) {
    const n = nums.length;
    const dp = Array(n).fill(false);
    dp[n - 1] = true;
    
    for (let i = n - 2; i >= 0; i--) {
        const end = Math.min(n, i + nums[i] + 1);
        for (let j = i + 1; j < end; j++) {
            if (dp[j]) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[0];
}
```

#### Java
```java
public boolean canJump(int[] nums) {
    int n = nums.length;
    boolean[] dp = new boolean[n];
    dp[n - 1] = true;
    
    for (int i = n - 2; i >= 0; i--) {
        int end = Math.min(n, i + nums[i] + 1);
        for (int j = i + 1; j < end; j++) {
            if (dp[j]) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[0];
}
```

### 4. Greedy
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def canJump(nums):
    goal = len(nums) - 1
    
    for i in range(len(nums) - 2, -1, -1):
        if i + nums[i] >= goal:
            goal = i
    
    return goal == 0
```

#### JavaScript
```javascript
function canJump(nums) {
    let goal = nums.length - 1;
    
    for (let i = nums.length - 2; i >= 0; i--) {
        if (i + nums[i] >= goal) {
            goal = i;
        }
    }
    
    return goal === 0;
}
```

#### Java
```java
public boolean canJump(int[] nums) {
    int goal = nums.length - 1;
    
    for (int i = nums.length - 2; i >= 0; i--) {
        if (i + nums[i] >= goal) {
            goal = i;
        }
    }
    
    return goal == 0;
}
```