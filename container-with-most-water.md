# Container With Most Water

## Problem Description
You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `i`th line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Note: You may not slant the container.

## Approaches

### 1. Brute Force
* Time complexity: O(n²)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        res = 0
        
        for i in range(len(height)):
            for j in range(i + 1, len(height)):
                # Calculate area: width × height
                # Width is distance between lines (j - i)
                # Height is limited by the shorter line
                area = (j - i) * min(height[i], height[j])
                res = max(res, area)
                
        return res
```

#### JavaScript
```javascript
var maxArea = function(height) {
    let maxWater = 0;
    
    for (let i = 0; i < height.length; i++) {
        for (let j = i + 1; j < height.length; j++) {
            // Calculate area: width × height
            // Width is distance between lines (j - i)
            // Height is limited by the shorter line
            const area = (j - i) * Math.min(height[i], height[j]);
            maxWater = Math.max(maxWater, area);
        }
    }
    
    return maxWater;
};
```

#### Java
```java
class Solution {
    public int maxArea(int[] height) {
        int maxWater = 0;
        
        for (int i = 0; i < height.length; i++) {
            for (int j = i + 1; j < height.length; j++) {
                // Calculate area: width × height
                // Width is distance between lines (j - i)
                // Height is limited by the shorter line
                int area = (j - i) * Math.min(height[i], height[j]);
                maxWater = Math.max(maxWater, area);
            }
        }
        
        return maxWater;
    }
}
```

### 2. Two Pointers (Optimal) ✅
* Time complexity: O(n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        l, r = 0, len(height) - 1
        res = 0
        
        while l < r:
            # Calculate area: width × height
            # Width is distance between lines (r - l)
            # Height is limited by the shorter line
            area = (r - l) * min(height[l], height[r])
            res = max(res, area)
            
            # Move the pointer at the shorter line
            if height[l] <= height[r]:
                l += 1
            else:
                r -= 1
                
        return res
```

#### JavaScript
```javascript
var maxArea = function(height) {
    let left = 0;
    let right = height.length - 1;
    let maxWater = 0;
    
    while (left < right) {
        // Calculate area: width × height
        // Width is distance between lines (right - left)
        // Height is limited by the shorter line
        const area = (right - left) * Math.min(height[left], height[right]);
        maxWater = Math.max(maxWater, area);
        
        // Move the pointer at the shorter line
        if (height[left] <= height[right]) {
            left++;
        } else {
            right--;
        }
    }
    
    return maxWater;
};
```

#### Java
```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int maxWater = 0;
        
        while (left < right) {
            // Calculate area: width × height
            // Width is distance between lines (right - left)
            // Height is limited by the shorter line
            int area = (right - left) * Math.min(height[left], height[right]);
            maxWater = Math.max(maxWater, area);
            
            // Move the pointer at the shorter line
            if (height[left] <= height[right]) {
                left++;
            } else {
                right--;
            }
        }
        
        return maxWater;
    }
}
```
