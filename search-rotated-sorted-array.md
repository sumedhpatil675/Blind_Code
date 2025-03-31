# Search in Rotated Sorted Array

## Problem Description
There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k` (1 <= k < nums.length) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed).

For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index 3 and become `[4,5,6,7,0,1,2]`.

Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with O(log n) runtime complexity.

## Approaches

### 1. Linear Search
* Time complexity: O(n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        for i in range(len(nums)):
            if nums[i] == target:
                return i
        return -1
```

#### JavaScript
```javascript
var search = function(nums, target) {
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] === target) {
            return i;
        }
    }
    return -1;
};
```

#### Java
```java
class Solution {
    public int search(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) {
                return i;
            }
        }
        return -1;
    }
}
```

### 2. Find Pivot then Binary Search
* Time complexity: O(log n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        # First find the pivot point (smallest element)
        left, right = 0, len(nums) - 1
        
        while left < right:
            mid = (left + right) // 2
            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid
                
        # 'left' is now the index of the smallest element
        pivot = left
        
        # Perform binary search on the appropriate half
        if target >= nums[pivot] and target <= nums[-1]:
            # Search in the right half
            left, right = pivot, len(nums) - 1
        else:
            # Search in the left half
            left, right = 0, pivot - 1
            
        # Standard binary search
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
                
        return -1
```

#### JavaScript
```javascript
var search = function(nums, target) {
    // First find the pivot point (smallest element)
    let left = 0;
    let right = nums.length - 1;
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (nums[mid] > nums[right]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    // 'left' is now the index of the smallest element
    const pivot = left;
    
    // Perform binary search on the appropriate half
    if (target >= nums[pivot] && target <= nums[nums.length - 1]) {
        // Search in the right half
        left = pivot;
        right = nums.length - 1;
    } else {
        // Search in the left half
        left = 0;
        right = pivot - 1;
    }
    
    // Standard binary search
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (nums[mid] === target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
};
```

#### Java
```java
class Solution {
    public int search(int[] nums, int target) {
        // First find the pivot point (smallest element)
        int left = 0;
        int right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        // 'left' is now the index of the smallest element
        int pivot = left;
        
        // Perform binary search on the appropriate half
        if (pivot == 0 || (target >= nums[pivot] && target <= nums[nums.length - 1])) {
            // Search in the right half
            left = pivot;
            right = nums.length - 1;
        } else {
            // Search in the left half
            left = 0;
            right = pivot - 1;
        }
        
        // Standard binary search
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return -1;
    }
}
```

### 3. One-Pass Binary Search
* Time complexity: O(log n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        
        while left <= right:
            mid = (left + right) // 2
            
            if nums[mid] == target:
                return mid
                
            # Check if left half is sorted
            if nums[left] <= nums[mid]:
                # Check if target is in the left half
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            # Right half is sorted
            else:
                # Check if target is in the right half
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
                    
        return -1
```

#### JavaScript
```javascript
var search = function(nums, target) {
    let left = 0;
    let right = nums.length - 1;
    
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        
        if (nums[mid] === target) {
            return mid;
        }
        
        // Check if left half is sorted
        if (nums[left] <= nums[mid]) {
            // Check if target is in the left half
            if (nums[left] <= target && target < nums[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        // Right half is sorted
        else {
            // Check if target is in the right half
            if (nums[mid] < target && target <= nums[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;
};
```

#### Java
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                return mid;
            }
            
            // Check if left half is sorted
            if (nums[left] <= nums[mid]) {
                // Check if target is in the left half
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            // Right half is sorted
            else {
                // Check if target is in the right half
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        
        return -1;
    }
}
```