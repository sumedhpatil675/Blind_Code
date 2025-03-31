# Find Minimum in Rotated Sorted Array

## Problem Description
Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:
- `[4,5,6,7,0,1,2]` if it was rotated 4 times.
- `[0,1,2,4,5,6,7]` if it was rotated 7 times.

Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

## Approaches

### 1. Linear Search
* Time complexity: O(n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        return min(nums)
```

#### JavaScript
```javascript
var findMin = function(nums) {
    return Math.min(...nums);
};
```

#### Java
```java
class Solution {
    public int findMin(int[] nums) {
        int min = nums[0];
        for (int num : nums) {
            min = Math.min(min, num);
        }
        return min;
    }
}
```

### 2. Binary Search
* Time complexity: O(log n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        # Initialize result with the first element
        res = nums[0]
        
        # Binary search
        left, right = 0, len(nums) - 1
        
        while left <= right:
            # If the array is already sorted in this window
            if nums[left] < nums[right]:
                res = min(res, nums[left])
                break
                
            # Calculate the middle index
            mid = (left + right) // 2
            res = min(res, nums[mid])
            
            # Determine which half to search
            if nums[mid] >= nums[left]:
                # The minimum must be in the right half
                left = mid + 1
            else:
                # The minimum must be in the left half
                right = mid - 1
                
        return res
```

#### JavaScript
```javascript
var findMin = function(nums) {
    // Initialize result with the first element
    let result = nums[0];
    
    // Binary search
    let left = 0;
    let right = nums.length - 1;
    
    while (left <= right) {
        // If the array is already sorted in this window
        if (nums[left] < nums[right]) {
            result = Math.min(result, nums[left]);
            break;
        }
        
        // Calculate the middle index
        const mid = Math.floor((left + right) / 2);
        result = Math.min(result, nums[mid]);
        
        // Determine which half to search
        if (nums[mid] >= nums[left]) {
            // The minimum must be in the right half
            left = mid + 1;
        } else {
            // The minimum must be in the left half
            right = mid - 1;
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int findMin(int[] nums) {
        // Initialize result with the first element
        int result = nums[0];
        
        // Binary search
        int left = 0;
        int right = nums.length - 1;
        
        while (left <= right) {
            // If the array is already sorted in this window
            if (nums[left] < nums[right]) {
                result = Math.min(result, nums[left]);
                break;
            }
            
            // Calculate the middle index
            int mid = left + (right - left) / 2;
            result = Math.min(result, nums[mid]);
            
            // Determine which half to search
            if (nums[mid] >= nums[left]) {
                // The minimum must be in the right half
                left = mid + 1;
            } else {
                // The minimum must be in the left half
                right = mid - 1;
            }
        }
        
        return result;
    }
}
```

### 3. Binary Search (Optimized)
* Time complexity: O(log n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1
        
        while left < right:
            mid = left + (right - left) // 2
            
            # If mid element is greater than the rightmost element,
            # the minimum must be in the right half
            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                # The minimum is either at mid or in the left half
                right = mid
                
        return nums[left]
```

#### JavaScript
```javascript
var findMin = function(nums) {
    let left = 0;
    let right = nums.length - 1;
    
    while (left < right) {
        const mid = Math.floor(left + (right - left) / 2);
        
        // If mid element is greater than the rightmost element,
        // the minimum must be in the right half
        if (nums[mid] > nums[right]) {
            left = mid + 1;
        } else {
            // The minimum is either at mid or in the left half
            right = mid;
        }
    }
    
    return nums[left];
};
```

#### Java
```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            // If mid element is greater than the rightmost element,
            // the minimum must be in the right half
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                // The minimum is either at mid or in the left half
                right = mid;
            }
        }
        
        return nums[left];
    }
}
```