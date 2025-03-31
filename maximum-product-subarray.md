# Maximum Product Subarray

Given an integer array nums, find a subarray that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a 32-bit integer.

## Approaches

### 1. Brute Force

**Time complexity:** O(n²)  
**Space complexity:** O(1)

#### Python
```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        res = nums[0]
        
        for i in range(len(nums)):
            cur = nums[i]
            res = max(res, cur)
            
            for j in range(i + 1, len(nums)):
                cur *= nums[j]
                res = max(res, cur)
                
        return res
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxProduct = function(nums) {
    let res = nums[0];
    
    for (let i = 0; i < nums.length; i++) {
        let cur = nums[i];
        res = Math.max(res, cur);
        
        for (let j = i + 1; j < nums.length; j++) {
            cur *= nums[j];
            res = Math.max(res, cur);
        }
    }
    
    return res;
};
```

#### Java
```java
class Solution {
    public int maxProduct(int[] nums) {
        int res = nums[0];
        
        for (int i = 0; i < nums.length; i++) {
            int cur = nums[i];
            res = Math.max(res, cur);
            
            for (int j = i + 1; j < nums.length; j++) {
                cur *= nums[j];
                res = Math.max(res, cur);
            }
        }
        
        return res;
    }
}
```

### 2. Sliding Window

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        A = []
        cur = []
        res = float('-inf')
        
        for num in nums:
            res = max(res, num)
            
            if num == 0:
                if cur:
                    A.append(cur)
                cur = []
            else:
                cur.append(num)
                
        if cur:
            A.append(cur)
            
        for sub in A:
            negs = sum(1 for i in sub if i < 0)
            prod = 1
            need = negs if negs % 2 == 0 else negs - 1
            negs = 0
            j = 0
            
            for i in range(len(sub)):
                prod *= sub[i]
                
                if sub[i] < 0:
                    negs += 1
                    
                while negs > need:
                    prod //= sub[j]
                    if sub[j] < 0:
                        negs -= 1
                    j += 1
                    
                if j <= i:
                    res = max(res, prod)
                    
        return res
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxProduct = function(nums) {
    // Handle subarrays separated by zeros
    const subarrays = [];
    let currentSubarray = [];
    let result = Math.max(...nums); // Handle single element case
    
    for (const num of nums) {
        if (num === 0) {
            if (currentSubarray.length > 0) {
                subarrays.push(currentSubarray);
                currentSubarray = [];
            }
        } else {
            currentSubarray.push(num);
        }
    }
    
    if (currentSubarray.length > 0) {
        subarrays.push(currentSubarray);
    }
    
    for (const subarray of subarrays) {
        const negativeCount = subarray.filter(num => num < 0).length;
        let product = 1;
        
        // If even number of negatives, we can use all elements
        if (negativeCount % 2 === 0) {
            for (const num of subarray) {
                product *= num;
            }
            result = Math.max(result, product);
        } else {
            // If odd number of negatives, we need to try removing first or last negative
            let prefixProduct = 1;
            let suffixProduct = 1;
            let firstNegativeIdx = -1;
            let lastNegativeIdx = -1;
            
            for (let i = 0; i < subarray.length; i++) {
                if (subarray[i] < 0) {
                    if (firstNegativeIdx === -1) {
                        firstNegativeIdx = i;
                    }
                    lastNegativeIdx = i;
                }
            }
            
            // Try removing first negative
            for (let i = firstNegativeIdx + 1; i < subarray.length; i++) {
                prefixProduct *= subarray[i];
            }
            
            // Try removing last negative
            for (let i = 0; i < lastNegativeIdx; i++) {
                suffixProduct *= subarray[i];
            }
            
            // Calculate full product
            let fullProduct = 1;
            for (const num of subarray) {
                fullProduct *= num;
            }
            
            result = Math.max(result, fullProduct);
            if (prefixProduct !== 1) result = Math.max(result, prefixProduct);
            if (suffixProduct !== 1) result = Math.max(result, suffixProduct);
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int maxProduct(int[] nums) {
        // Handle subarrays separated by zeros
        List<List<Integer>> subarrays = new ArrayList<>();
        List<Integer> currentSubarray = new ArrayList<>();
        int result = Integer.MIN_VALUE;
        
        for (int num : nums) {
            result = Math.max(result, num); // Handle single element case
            
            if (num == 0) {
                if (!currentSubarray.isEmpty()) {
                    subarrays.add(new ArrayList<>(currentSubarray));
                    currentSubarray.clear();
                }
            } else {
                currentSubarray.add(num);
            }
        }
        
        if (!currentSubarray.isEmpty()) {
            subarrays.add(currentSubarray);
        }
        
        for (List<Integer> subarray : subarrays) {
            int negCount = 0;
            for (int num : subarray) {
                if (num < 0) negCount++;
            }
            
            int product = 1;
            for (int num : subarray) {
                product *= num;
            }
            
            if (negCount % 2 == 0) {
                // Even number of negative numbers
                result = Math.max(result, product);
            } else {
                // Odd number of negative numbers
                // Try to remove either the first or last negative number
                int leftProduct = product;
                int rightProduct = product;
                
                for (int i = 0; i < subarray.size(); i++) {
                    if (subarray.get(i) < 0) {
                        leftProduct /= subarray.get(i);
                        break;
                    }
                }
                
                for (int i = subarray.size() - 1; i >= 0; i--) {
                    if (subarray.get(i) < 0) {
                        rightProduct /= subarray.get(i);
                        break;
                    }
                }
                
                result = Math.max(result, Math.max(leftProduct, rightProduct));
            }
        }
        
        return result;
    }
}
```

### 3. Kadane's Algorithm

**Time complexity:** O(n)  
**Space complexity:** O(1)

#### Python
```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        res = nums[0]
        curMin, curMax = 1, 1
        
        for num in nums:
            tmp = curMax * num
            curMax = max(num * curMax, num * curMin, num)
            curMin = min(tmp, num * curMin, num)
            res = max(res, curMax)
            
        return res
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxProduct = function(nums) {
    if (nums.length === 0) return 0;
    
    let result = nums[0];
    let currentMax = 1;
    let currentMin = 1;
    
    for (const num of nums) {
        // Need to store currentMax before updating it
        const temp = currentMax * num;
        
        // Update max and min products for current position
        currentMax = Math.max(num, num * currentMax, num * currentMin);
        currentMin = Math.min(num, temp, num * currentMin);
        
        // Update global max
        result = Math.max(result, currentMax);
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums.length == 0) return 0;
        
        int result = nums[0];
        int currentMax = 1;
        int currentMin = 1;
        
        for (int num : nums) {
            // Need to store currentMax before updating it
            int temp = currentMax * num;
            
            // Update max and min products for current position
            currentMax = Math.max(num, Math.max(num * currentMax, num * currentMin));
            currentMin = Math.min(num, Math.min(temp, num * currentMin));
            
            // Update global max
            result = Math.max(result, currentMax);
        }
        
        return result;
    }
}
```

### 4. Prefix & Suffix

**Time complexity:** O(n)  
**Space complexity:** O(1)

#### Python
```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        n, res = len(nums), nums[0]
        
        prefix = suffix = 0
        
        for i in range(n):
            prefix = nums[i] * (prefix or 1)
            suffix = nums[n - 1 - i] * (suffix or 1)
            res = max(res, max(prefix, suffix))
            
        return res
```

#### JavaScript
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxProduct = function(nums) {
    const n = nums.length;
    let result = nums[0];
    let prefix = 0;
    let suffix = 0;
    
    for (let i = 0; i < n; i++) {
        // Reset prefix and suffix if they become 0
        prefix = prefix === 0 ? 1 : prefix;
        suffix = suffix === 0 ? 1 : suffix;
        
        // Update prefix and suffix
        prefix *= nums[i];
        suffix *= nums[n - 1 - i];
        
        // Update result
        result = Math.max(result, Math.max(prefix, suffix));
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int result = nums[0];
        int prefix = 0;
        int suffix = 0;
        
        for (int i = 0; i < n; i++) {
            // Reset prefix and suffix if they become 0
            prefix = prefix == 0 ? 1 : prefix;
            suffix = suffix == 0 ? 1 : suffix;
            
            // Update prefix and suffix
            prefix *= nums[i];
            suffix *= nums[n - 1 - i];
            
            // Update result
            result = Math.max(result, Math.max(prefix, suffix));
        }
        
        return result;
    }
}
```