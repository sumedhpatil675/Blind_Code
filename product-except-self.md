# Product of Array Except Self

## Problem Description
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer. You must write an algorithm that runs in O(n) time and without using the division operation.

## Approaches

### 1. Brute Force
* Time complexity: O(n²)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        res = [0] * n
        
        for i in range(n):
            prod = 1
            for j in range(n):
                if i == j:
                    continue
                prod *= nums[j]
            res[i] = prod
            
        return res
```

#### JavaScript
```javascript
var productExceptSelf = function(nums) {
    const n = nums.length;
    const result = new Array(n).fill(0);
    
    for (let i = 0; i < n; i++) {
        let product = 1;
        for (let j = 0; j < n; j++) {
            if (i === j) continue;
            product *= nums[j];
        }
        result[i] = product;
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        
        for (int i = 0; i < n; i++) {
            int product = 1;
            for (int j = 0; j < n; j++) {
                if (i == j) continue;
                product *= nums[j];
            }
            result[i] = product;
        }
        
        return result;
    }
}
```

### 2. Division (Not allowed in this problem)
* Time complexity: O(n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        prod, zero_cnt = 1, 0
        
        for num in nums:
            if num:
                prod *= num
            else:
                zero_cnt += 1
                
        if zero_cnt > 1:
            return [0] * len(nums)
        
        res = [0] * len(nums)
        
        for i, c in enumerate(nums):
            if zero_cnt:
                res[i] = 0 if c else prod
            else:
                res[i] = prod // c
                
        return res
```

#### JavaScript
```javascript
var productExceptSelf = function(nums) {
    let product = 1;
    let zeroCount = 0;
    
    // Calculate product of all non-zero numbers and count zeros
    for (const num of nums) {
        if (num !== 0) {
            product *= num;
        } else {
            zeroCount++;
        }
    }
    
    // Handle result
    const result = [];
    
    if (zeroCount > 1) {
        // If more than one zero, everything is 0
        return new Array(nums.length).fill(0);
    }
    
    for (const num of nums) {
        if (zeroCount) {
            // If there's one zero, only non-zero elements get 0
            result.push(num === 0 ? product : 0);
        } else {
            // If no zeros, divide product by current number
            result.push(product / num);
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int prod = 1;
        int zeroCnt = 0;
        
        for (int num : nums) {
            if (num != 0) {
                prod *= num;
            } else {
                zeroCnt++;
            }
        }
        
        int[] res = new int[nums.length];
        
        if (zeroCnt > 1) {
            // All elements will be 0
            return res;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (zeroCnt > 0) {
                res[i] = nums[i] == 0 ? prod : 0;
            } else {
                res[i] = prod / nums[i];
            }
        }
        
        return res;
    }
}
```

### 3. Prefix & Suffix Product Arrays
* Time complexity: O(n)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        res = [0] * n
        
        # Initialize prefix and suffix arrays
        pref = [0] * n
        suff = [0] * n
        
        # Calculate prefix products
        pref[0] = 1
        for i in range(1, n):
            pref[i] = nums[i - 1] * pref[i - 1]
        
        # Calculate suffix products
        suff[n - 1] = 1
        for i in range(n - 2, -1, -1):
            suff[i] = nums[i + 1] * suff[i + 1]
        
        # Combine prefix and suffix products
        for i in range(n):
            res[i] = pref[i] * suff[i]
            
        return res
```

#### JavaScript
```javascript
var productExceptSelf = function(nums) {
    const n = nums.length;
    const result = new Array(n);
    
    // Initialize prefix and suffix arrays
    const prefix = new Array(n);
    const suffix = new Array(n);
    
    // Calculate prefix products
    prefix[0] = 1;
    for (let i = 1; i < n; i++) {
        prefix[i] = nums[i - 1] * prefix[i - 1];
    }
    
    // Calculate suffix products
    suffix[n - 1] = 1;
    for (let i = n - 2; i >= 0; i--) {
        suffix[i] = nums[i + 1] * suffix[i + 1];
    }
    
    // Combine prefix and suffix products
    for (let i = 0; i < n; i++) {
        result[i] = prefix[i] * suffix[i];
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];
        
        // Initialize prefix and suffix arrays
        int[] pref = new int[n];
        int[] suff = new int[n];
        
        // Calculate prefix products
        pref[0] = 1;
        for (int i = 1; i < n; i++) {
            pref[i] = nums[i - 1] * pref[i - 1];
        }
        
        // Calculate suffix products
        suff[n - 1] = 1;
        for (int i = n - 2; i >= 0; i--) {
            suff[i] = nums[i + 1] * suff[i + 1];
        }
        
        // Combine prefix and suffix products
        for (int i = 0; i < n; i++) {
            res[i] = pref[i] * suff[i];
        }
        
        return res;
    }
}
```

### 4. Prefix & Suffix (Space Optimized)
* Time complexity: O(n)
* Space complexity: O(1) (excluding output array)

#### Python
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        res = [1] * n
        
        # Calculate prefix products in-place
        prefix = 1
        for i in range(n):
            res[i] = prefix
            prefix *= nums[i]
        
        # Calculate suffix products and multiply with prefix products
        postfix = 1
        for i in range(n - 1, -1, -1):
            res[i] *= postfix
            postfix *= nums[i]
            
        return res
```

#### JavaScript
```javascript
var productExceptSelf = function(nums) {
    const n = nums.length;
    const result = new Array(n).fill(1);
    
    // Calculate prefix products in-place
    let prefix = 1;
    for (let i = 0; i < n; i++) {
        result[i] = prefix;
        prefix *= nums[i];
    }
    
    // Calculate suffix products and multiply with prefix products
    let postfix = 1;
    for (let i = n - 1; i >= 0; i--) {
        result[i] *= postfix;
        postfix *= nums[i];
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];
        
        // Fill the res array with 1s
        Arrays.fill(res, 1);
        
        // Calculate prefix products in-place
        int prefix = 1;
        for (int i = 0; i < n; i++) {
            res[i] = prefix;
            prefix *= nums[i];
        }
        
        // Calculate suffix products and multiply with prefix products
        int postfix = 1;
        for (int i = n - 1; i >= 0; i--) {
            res[i] *= postfix;
            postfix *= nums[i];
        }
        
        return res;
    }
}
```