# Maximum Subarray Sum

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

## Approaches

### 1. Brute Force
- **Time complexity**: O(n²)
- **Space complexity**: O(1)

#### Python
```python
def maxSubArray(nums):
    max_sum = float('-inf')
    for i in range(len(nums)):
        current_sum = 0
        for j in range(i, len(nums)):
            current_sum += nums[j]
            max_sum = max(max_sum, current_sum)
    return max_sum
```

#### JavaScript
```javascript
function maxSubArray(nums) {
    let maxSum = -Infinity;
    for (let i = 0; i < nums.length; i++) {
        let currentSum = 0;
        for (let j = i; j < nums.length; j++) {
            currentSum += nums[j];
            maxSum = Math.max(maxSum, currentSum);
        }
    }
    return maxSum;
}
```

#### Java
```java
public int maxSubArray(int[] nums) {
    int maxSum = Integer.MIN_VALUE;
    for (int i = 0; i < nums.length; i++) {
        int currentSum = 0;
        for (int j = i; j < nums.length; j++) {
            currentSum += nums[j];
            maxSum = Math.max(maxSum, currentSum);
        }
    }
    return maxSum;
}
```

### 2. Kadane's Algorithm
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def maxSubArray(self, nums: List[int]) -> int:
    maxSub = nums[0]
    currSum = 0
    for i in range(len(nums)):
        if currSum < 0:
            currSum = 0
        currSum += nums[i]
        maxSub = max(maxSub,currSum)
    return maxSub
```

#### JavaScript
```javascript
function maxSubArray(nums) {
    let maxSub = nums[0];
    let currSum = 0;
    for (let i = 0; i < nums.length; i++) {
        if (currSum < 0) {
            currSum = 0;
        }
        currSum += nums[i];
        maxSub = Math.max(maxSub, currSum);
    }
    return maxSub;
}
```

#### Java
```java
public int maxSubArray(int[] nums) {
    int maxSub = nums[0];
    int currSum = 0;
    for (int i = 0; i < nums.length; i++) {
        if (currSum < 0) {
            currSum = 0;
        }
        currSum += nums[i];
        maxSub = Math.max(maxSub, currSum);
    }
    return maxSub;
}
```

### 3. Dynamic Programming
- **Time complexity**: O(n)
- **Space complexity**: O(n)

#### Python
```python
def maxSubArray(nums):
    # dp[i] represents the maximum subarray sum ending at index i
    dp = [0] * len(nums)
    dp[0] = nums[0]
    
    for i in range(1, len(nums)):
        dp[i] = max(nums[i], dp[i-1] + nums[i])
    
    return max(dp)
```

#### JavaScript
```javascript
function maxSubArray(nums) {
    // dp[i] represents the maximum subarray sum ending at index i
    const dp = Array(nums.length).fill(0);
    dp[0] = nums[0];
    
    for (let i = 1; i < nums.length; i++) {
        dp[i] = Math.max(nums[i], dp[i-1] + nums[i]);
    }
    
    return Math.max(...dp);
}
```

#### Java
```java
public int maxSubArray(int[] nums) {
    // dp[i] represents the maximum subarray sum ending at index i
    int[] dp = new int[nums.length];
    dp[0] = nums[0];
    int maxSum = dp[0];
    
    for (int i = 1; i < nums.length; i++) {
        dp[i] = Math.max(nums[i], dp[i-1] + nums[i]);
        maxSum = Math.max(maxSum, dp[i]);
    }
    
    return maxSum;
}
```

### 4. Divide and Conquer
- **Time complexity**: O(n log n)
- **Space complexity**: O(log n) due to recursion stack

#### Python
```python
def maxSubArray(nums):
    def divide_and_conquer(left, right):
        if left == right:
            return nums[left]
        
        mid = (left + right) // 2
        
        # Find the maximum subarray sum in the left half
        left_max = divide_and_conquer(left, mid)
        
        # Find the maximum subarray sum in the right half
        right_max = divide_and_conquer(mid + 1, right)
        
        # Find the maximum subarray sum that crosses the midpoint
        left_sum = float('-inf')
        right_sum = float('-inf')
        current_sum = 0
        
        # Maximum sum starting from mid and going left
        for i in range(mid, left - 1, -1):
            current_sum += nums[i]
            left_sum = max(left_sum, current_sum)
        
        current_sum = 0
        
        # Maximum sum starting from mid+1 and going right
        for i in range(mid + 1, right + 1):
            current_sum += nums[i]
            right_sum = max(right_sum, current_sum)
        
        # Return the maximum of the three scenarios
        cross_sum = left_sum + right_sum
        return max(left_max, right_max, cross_sum)
    
    return divide_and_conquer(0, len(nums) - 1)
```

#### JavaScript
```javascript
function maxSubArray(nums) {
    function divideAndConquer(left, right) {
        if (left === right) {
            return nums[left];
        }
        
        const mid = Math.floor((left + right) / 2);
        
        // Find the maximum subarray sum in the left half
        const leftMax = divideAndConquer(left, mid);
        
        // Find the maximum subarray sum in the right half
        const rightMax = divideAndConquer(mid + 1, right);
        
        // Find the maximum subarray sum that crosses the midpoint
        let leftSum = -Infinity;
        let rightSum = -Infinity;
        let currentSum = 0;
        
        // Maximum sum starting from mid and going left
        for (let i = mid; i >= left; i--) {
            currentSum += nums[i];
            leftSum = Math.max(leftSum, currentSum);
        }
        
        currentSum = 0;
        
        // Maximum sum starting from mid+1 and going right
        for (let i = mid + 1; i <= right; i++) {
            currentSum += nums[i];
            rightSum = Math.max(rightSum, currentSum);
        }
        
        // Return the maximum of the three scenarios
        const crossSum = leftSum + rightSum;
        return Math.max(leftMax, rightMax, crossSum);
    }
    
    return divideAndConquer(0, nums.length - 1);
}
```

#### Java
```java
public int maxSubArray(int[] nums) {
    return divideAndConquer(nums, 0, nums.length - 1);
}

private int divideAndConquer(int[] nums, int left, int right) {
    if (left == right) {
        return nums[left];
    }
    
    int mid = (left + right) / 2;
    
    // Find the maximum subarray sum in the left half
    int leftMax = divideAndConquer(nums, left, mid);
    
    // Find the maximum subarray sum in the right half
    int rightMax = divideAndConquer(nums, mid + 1, right);
    
    // Find the maximum subarray sum that crosses the midpoint
    int leftSum = Integer.MIN_VALUE;
    int rightSum = Integer.MIN_VALUE;
    int currentSum = 0;
    
    // Maximum sum starting from mid and going left
    for (int i = mid; i >= left; i--) {
        currentSum += nums[i];
        leftSum = Math.max(leftSum, currentSum);
    }
    
    currentSum = 0;
    
    // Maximum sum starting from mid+1 and going right
    for (int i = mid + 1; i <= right; i++) {
        currentSum += nums[i];
        rightSum = Math.max(rightSum, currentSum);
    }
    
    // Return the maximum of the three scenarios
    int crossSum = leftSum + rightSum;
    return Math.max(Math.max(leftMax, rightMax), crossSum);
}
```
