# House Robber II

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

## Approaches

### 1. Recursion

**Time complexity:** O(2^n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]
            
        def dfs(i, flag):
            if i >= len(nums) or (flag and i == len(nums) - 1):
                return 0
                
            return max(dfs(i + 1, flag), nums[i] + dfs(i + 2, flag or i == 0))
            
        return max(dfs(0, True), dfs(1, False))
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    if (nums.length === 1) {
        return nums[0];
    }
    
    function dfs(i, flag) {
        if (i >= nums.length || (flag && i === nums.length - 1)) {
            return 0;
        }
        
        return Math.max(
            dfs(i + 1, flag),
            nums[i] + dfs(i + 2, flag || i === 0)
        );
    }
    
    return Math.max(dfs(0, true), dfs(1, false));
};
```

#### Java
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }
        
        return Math.max(robHelper(nums, 0, true), robHelper(nums, 1, false));
    }
    
    private int robHelper(int[] nums, int i, boolean flag) {
        if (i >= nums.length || (flag && i == nums.length - 1)) {
            return 0;
        }
        
        return Math.max(
            robHelper(nums, i + 1, flag),
            nums[i] + robHelper(nums, i + 2, flag || i == 0)
        );
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
        if len(nums) == 1:
            return nums[0]
            
        memo = [[-1] * 2 for _ in range(len(nums))]
        
        def dfs(i, flag):
            if i >= len(nums) or (flag and i == len(nums) - 1):
                return 0
                
            if memo[i][flag] != -1:
                return memo[i][flag]
                
            memo[i][flag] = max(dfs(i + 1, flag), nums[i] + dfs(i + 2, flag or (i == 0)))
            return memo[i][flag]
            
        return max(dfs(0, True), dfs(1, False))
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    if (nums.length === 1) {
        return nums[0];
    }
    
    const memo = Array(nums.length).fill().map(() => Array(2).fill(-1));
    
    function dfs(i, flag) {
        if (i >= nums.length || (flag && i === nums.length - 1)) {
            return 0;
        }
        
        if (memo[i][flag ? 1 : 0] !== -1) {
            return memo[i][flag ? 1 : 0];
        }
        
        memo[i][flag ? 1 : 0] = Math.max(
            dfs(i + 1, flag),
            nums[i] + dfs(i + 2, flag || i === 0)
        );
        
        return memo[i][flag ? 1 : 0];
    }
    
    return Math.max(dfs(0, true), dfs(1, false));
};
```

#### Java
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }
        
        int[][] memo = new int[nums.length][2];
        for (int[] row : memo) {
            Arrays.fill(row, -1);
        }
        
        return Math.max(
            robHelper(nums, 0, true, memo),
            robHelper(nums, 1, false, memo)
        );
    }
    
    private int robHelper(int[] nums, int i, boolean flag, int[][] memo) {
        if (i >= nums.length || (flag && i == nums.length - 1)) {
            return 0;
        }
        
        if (memo[i][flag ? 1 : 0] != -1) {
            return memo[i][flag ? 1 : 0];
        }
        
        memo[i][flag ? 1 : 0] = Math.max(
            robHelper(nums, i + 1, flag, memo),
            nums[i] + robHelper(nums, i + 2, flag || i == 0, memo)
        );
        
        return memo[i][flag ? 1 : 0];
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
        if len(nums) == 1:
            return nums[0]
            
        return max(self.helper(nums[1:]), self.helper(nums[:-1]))
        
    def helper(self, nums: List[int]) -> int:
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
    if (nums.length === 1) {
        return nums[0];
    }
    
    return Math.max(
        helper(nums.slice(1)),
        helper(nums.slice(0, -1))
    );
    
    function helper(nums) {
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
    }
};
```

#### Java
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }
        
        return Math.max(
            helper(Arrays.copyOfRange(nums, 1, nums.length)),
            helper(Arrays.copyOfRange(nums, 0, nums.length - 1))
        );
    }
    
    private int helper(int[] nums) {
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
        return max(nums[0], self.helper(nums[1:]), self.helper(nums[:-1]))
        
    def helper(self, nums):
        rob1, rob2 = 0, 0
        
        for num in nums:
            newRob = max(rob1 + num, rob2)
            rob1 = rob2
            rob2 = newRob
            
        return rob2
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    if (nums.length === 1) {
        return nums[0];
    }
    
    return Math.max(
        helper(nums.slice(1)),
        helper(nums.slice(0, -1))
    );
    
    function helper(nums) {
        let rob1 = 0;
        let rob2 = 0;
        
        for (const num of nums) {
            const temp = Math.max(rob1 + num, rob2);
            rob1 = rob2;
            rob2 = temp;
        }
        
        return rob2;
    }
};
```

#### Java
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }
        
        return Math.max(
            helper(Arrays.copyOfRange(nums, 1, nums.length)),
            helper(Arrays.copyOfRange(nums, 0, nums.length - 1))
        );
    }
    
    private int helper(int[] nums) {
        int rob1 = 0;
        int rob2 = 0;
        
        for (int num : nums) {
            int temp = Math.max(rob1 + num, rob2);
            rob1 = rob2;
            rob2 = temp;
        }
        
        return rob2;
    }
}
```