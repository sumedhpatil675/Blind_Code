# Two Sum

## Problem Description
Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

## Approaches

### 1. Brute Force
* Time complexity: O(n²)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
        return []
```

#### JavaScript
```javascript
var twoSum = function(nums, target) {
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] === target) {
                return [i, j];
            }
        }
    }
    return [];
};
```

#### Java
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[] {i, j};
                }
            }
        }
        return new int[] {};
    }
}
```

### 2. Sorting with Two Pointers
* Time complexity: O(n log n)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # Create a list of [value, index] pairs
        A = []
        for i, num in enumerate(nums):
            A.append([num, i])
        
        # Sort by value
        A.sort()
        
        # Two pointer approach
        i, j = 0, len(nums) - 1
        while i < j:
            cur = A[i][0] + A[j][0]
            if cur == target:
                return [A[i][1], A[j][1]]
            elif cur < target:
                i += 1
            else:
                j -= 1
                
        return []
```

#### JavaScript
```javascript
var twoSum = function(nums, target) {
    // Create array of [value, index] pairs
    const pairs = nums.map((num, idx) => [num, idx]);
    
    // Sort by value
    pairs.sort((a, b) => a[0] - b[0]);
    
    // Two pointer approach
    let left = 0, right = pairs.length - 1;
    
    while (left < right) {
        const sum = pairs[left][0] + pairs[right][0];
        
        if (sum === target) {
            return [pairs[left][1], pairs[right][1]];
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    
    return [];
};
```

#### Java
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // Create array of [value, index] pairs
        int[][] pairs = new int[nums.length][2];
        for (int i = 0; i < nums.length; i++) {
            pairs[i][0] = nums[i];
            pairs[i][1] = i;
        }
        
        // Sort by value
        Arrays.sort(pairs, (a, b) -> Integer.compare(a[0], b[0]));
        
        // Two pointer approach
        int left = 0, right = nums.length - 1;
        
        while (left < right) {
            int sum = pairs[left][0] + pairs[right][0];
            
            if (sum == target) {
                return new int[] {pairs[left][1], pairs[right][1]};
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        
        return new int[] {};
    }
}
```

### 3. Hash Map (Two Pass)
* Time complexity: O(n)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        indices = {}  # val -> index
        
        # First pass: build the hash map
        for i, n in enumerate(nums):
            indices[n] = i
            
        # Second pass: check for complements
        for i, n in enumerate(nums):
            diff = target - n
            if diff in indices and indices[diff] != i:
                return [i, indices[diff]]
```

#### JavaScript
```javascript
var twoSum = function(nums, target) {
    const indices = new Map();
    
    // First pass: build the hash map
    for (let i = 0; i < nums.length; i++) {
        indices.set(nums[i], i);
    }
    
    // Second pass: check for complements
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        if (indices.has(complement) && indices.get(complement) !== i) {
            return [i, indices.get(complement)];
        }
    }
    
    return [];
};
```

#### Java
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> indices = new HashMap<>();
        
        // First pass: build the hash map
        for (int i = 0; i < nums.length; i++) {
            indices.put(nums[i], i);
        }
        
        // Second pass: check for complements
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (indices.containsKey(complement) && indices.get(complement) != i) {
                return new int[] {i, indices.get(complement)};
            }
        }
        
        return new int[] {};
    }
}
```

### 4. Hash Map (One Pass)
* Time complexity: O(n)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        prevMap = {}  # val -> index
        
        for i, n in enumerate(nums):
            diff = target - n
            if diff in prevMap:
                return [prevMap[diff], i]
            prevMap[n] = i
```

#### JavaScript
```javascript
var twoSum = function(nums, target) {
    const prevMap = new Map();
    
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        
        if (prevMap.has(complement)) {
            return [prevMap.get(complement), i];
        }
        
        prevMap.set(nums[i], i);
    }
    
    return [];
};
```

#### Java
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> prevMap = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            
            if (prevMap.containsKey(complement)) {
                return new int[] {prevMap.get(complement), i};
            }
            
            prevMap.put(nums[i], i);
        }
        
        return new int[] {};
    }
}
```