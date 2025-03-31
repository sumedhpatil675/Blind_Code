# House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

## Approaches

### 1. Recursion

**Time complexity:** O(2^n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        def dfs(i):
            if i >= len(nums):
                return 0
                
            return max(dfs(i + 1), nums[i] + dfs(i + 2))
            
        return dfs(0)
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    function dfs(i) {
        if (i >= nums.length) {
            return 0;
        }
        
        return Math.max(dfs(i + 1), nums[i] + dfs(i + 2));
    }
    
    return dfs(0);
};
```

#### Java
```java
class Solution {
    public int rob(int[] nums) {
        return robHelper(nums, 0);
    }
    
    private int robHelper(int[] nums, int i) {
        if (i >= nums.length) {
            return 0;
        }
        
        return Math.max(robHelper(nums, i + 1), nums[i] + robHelper(nums, i + 2));
    }
}
```

### 2. Dynamic Programming (Top-Down)

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        memo = [-1] * len(nums)
        
        def dfs(i):
            if i >= len(nums):
                return 0
                
            if memo[i] != -1:
                return memo[i]
                
            memo[i] = max(dfs(i + 1), nums[i] + dfs(i + 2))
            return memo[i]
            
        return dfs(0)
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    const memo = Array(nums.length).fill(-1);
    
    function dfs(i) {
        if (i >= nums.length) {
            return 0;
        }
        
        if (memo[i] !== -1) {
            return memo[i];
        }
        
        memo[i] = Math.max(dfs(i + 1), nums[i] + dfs(i + 2));
        return memo[i];
    }
    
    return dfs(0);
};
```

#### Java
```java
class Solution {
    public int rob(int[] nums) {
        int[] memo = new int[nums.length];
        Arrays.fill(memo, -1);
        return robHelper(nums, 0, memo);
    }
    
    private int robHelper(int[] nums, int i, int[] memo) {
        if (i >= nums.length) {
            return 0;
        }
        
        if (memo[i] != -1) {
            return memo[i];
        }
        
        memo[i] = Math.max(robHelper(nums, i + 1, memo), 
                           nums[i] + robHelper(nums, i + 2, memo));
        return memo[i];
    }
}
```

### 3. Dynamic Programming (Bottom-Up)

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums:
            return 0
            
        if len(nums) == 1:
            return nums[0]
            
        dp = [0] * len(nums)
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])
        
        for i in range(2, len(nums)):
            dp[i] = max(dp[i - 1], nums[i] + dp[i - 2])
            
        return dp[-1]
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    if (nums.length === 0) {
        return 0;
    }
    
    if (nums.length === 1) {
        return nums[0];
    }
    
    const dp = Array(nums.length).fill(0);
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);
    
    for (let i = 2; i < nums.length; i++) {
        dp[i] = Math.max(dp[i - 1], nums[i] + dp[i - 2]);
    }
    
    return dp[nums.length - 1];
};
```

#### Java
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        
        if (nums.length == 1) {
            return nums[0];
        }
        
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        
        for (int i = 2; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 1], nums[i] + dp[i - 2]);
        }
        
        return dp[nums.length - 1];
    }
}
```

### 4. Dynamic Programming (Space Optimized)

**Time complexity:** O(n)  
**Space complexity:** O(1)

#### Python
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        rob1, rob2 = 0, 0
        
        for num in nums:
            temp = max(num + rob1, rob2)
            rob1 = rob2
            rob2 = temp
            
        return rob2
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    let rob1 = 0;
    let rob2 = 0;
    
    for (const num of nums) {
        const temp = Math.max(num + rob1, rob2);
        rob1 = rob2;
        rob2 = temp;
    }
    
    return rob2;
};
```

#### Java
```java
class Solution {
    public int rob(int[] nums) {
        int rob1 = 0;
        int rob2 = 0;
        
        for (int num : nums) {
            int temp = Math.max(num + rob1, rob2);
            rob1 = rob2;
            rob2 = temp;
        }
        
        return rob2;
    }
}
```