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

### 3. Dynamic Programming (Bottom-Up) ✅

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
        if len(nums) == 1:
            return nums[0]
            
        # Handle circular arrangement by creating two scenarios:
        # 1. Skip the first house
        # 2. Skip the last house
        leave_last_house = nums[:-1]
        leave_first_house = nums[1:]
        
        return max(self.helper(leave_last_house), self.helper(leave_first_house))
    
    def helper(self, nums: List[int]) -> int:
        one_before_previous_house_money, previous_house_money = 0, 0
        for i in range(len(nums)):
            current_house_money = nums[i]
            temp = max(one_before_previous_house_money + current_house_money, previous_house_money)
            one_before_previous_house_money = previous_house_money
            previous_house_money = temp
        return previous_house_money
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
    
    // Handle circular arrangement by creating two scenarios:
    // 1. Skip the first house
    // 2. Skip the last house
    const leave_last_house = nums.slice(0, nums.length - 1);
    const leave_first_house = nums.slice(1);
    
    return Math.max(helper(leave_last_house), helper(leave_first_house));
    
    function helper(nums) {
        let one_before_previous_house_money = 0;
        let previous_house_money = 0;
        
        for (let i = 0; i < nums.length; i++) {
            const current_house_money = nums[i];
            const temp = Math.max(one_before_previous_house_money + current_house_money, previous_house_money);
            one_before_previous_house_money = previous_house_money;
            previous_house_money = temp;
        }
        
        return previous_house_money;
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
        
        // Handle circular arrangement by creating two scenarios:
        // 1. Skip the first house
        // 2. Skip the last house
        return Math.max(helper(Arrays.copyOfRange(nums, 0, nums.length - 1)), 
                        helper(Arrays.copyOfRange(nums, 1, nums.length)));
    }
    
    private int helper(int[] nums) {
        int one_before_previous_house_money = 0;
        int previous_house_money = 0;
        
        for (int i = 0; i < nums.length; i++) {
            int current_house_money = nums[i];
            int temp = Math.max(one_before_previous_house_money + current_house_money, previous_house_money);
            one_before_previous_house_money = previous_house_money;
            previous_house_money = temp;
        }
        
        return previous_house_money;
    }
}
```
