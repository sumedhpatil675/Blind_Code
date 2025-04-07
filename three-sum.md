# Three Sum

## Problem Description
Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

## Approaches

### 1. Brute Force
* Time complexity: O(n³)
* Space complexity: O(n) for the result

#### Python
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = set()
        nums.sort()
        
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                for k in range(j + 1, len(nums)):
                    if nums[i] + nums[j] + nums[k] == 0:
                        res.add((nums[i], nums[j], nums[k]))
                        
        return [list(x) for x in res]
```

#### JavaScript
```javascript
var threeSum = function(nums) {
    const result = new Set();
    nums.sort((a, b) => a - b);
    
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            for (let k = j + 1; k < nums.length; k++) {
                if (nums[i] + nums[j] + nums[k] === 0) {
                    result.add(JSON.stringify([nums[i], nums[j], nums[k]]));
                }
            }
        }
    }
    
    return Array.from(result).map(triplet => JSON.parse(triplet));
};
```

#### Java
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> result = new HashSet<>();
        Arrays.sort(nums);
        
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> triplet = Arrays.asList(nums[i], nums[j], nums[k]);
                        result.add(triplet);
                    }
                }
            }
        }
        
        return new ArrayList<>(result);
    }
}
```

### 2. Hash Map Approach
* Time complexity: O(n²)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        result = []
        
        for i in range(len(nums) - 2):
            # Skip duplicates for i
            if i > 0 and nums[i] == nums[i-1]:
                continue
                
            # Two Sum II approach for the rest of the array
            seen = {}
            for j in range(i + 1, len(nums)):
                complement = -nums[i] - nums[j]
                if complement in seen:
                    result.append([nums[i], complement, nums[j]])
                    # Skip duplicates for j
                    while j + 1 < len(nums) and nums[j] == nums[j + 1]:
                        j += 1
                seen[nums[j]] = j
                
        return result
```

#### JavaScript
```javascript
var threeSum = function(nums) {
    nums.sort((a, b) => a - b);
    const result = [];
    
    for (let i = 0; i < nums.length - 2; i++) {
        // Skip duplicates for i
        if (i > 0 && nums[i] === nums[i-1]) {
            continue;
        }
        
        // Two Sum II approach for the rest of the array
        const seen = new Map();
        for (let j = i + 1; j < nums.length; j++) {
            const complement = -nums[i] - nums[j];
            if (seen.has(complement)) {
                result.push([nums[i], complement, nums[j]]);
                // Skip duplicates for j
                while (j + 1 < nums.length && nums[j] === nums[j + 1]) {
                    j++;
                }
            }
            seen.set(nums[j], j);
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        
        for (int i = 0; i < nums.length - 2; i++) {
            // Skip duplicates for i
            if (i > 0 && nums[i] == nums[i-1]) {
                continue;
            }
            
            // Two Sum II approach for the rest of the array
            Map<Integer, Integer> seen = new HashMap<>();
            for (int j = i + 1; j < nums.length; j++) {
                int complement = -nums[i] - nums[j];
                if (seen.containsKey(complement)) {
                    result.add(Arrays.asList(nums[i], complement, nums[j]));
                    // Skip duplicates for j
                    while (j + 1 < nums.length && nums[j] == nums[j + 1]) {
                        j++;
                    }
                }
                seen.put(nums[j], j);
            }
        }
        
        return result;
    }
}
```

### 3. Two Pointers (Optimal) ✅✅✅

* Time complexity: O(n²)
* Space complexity: O(1) excluding the output

## Python Implementation

```python
def threeSum(nums):
    res = []
    nums.sort()
    
    for i, a in enumerate(nums):
        # Skip duplicate values for the first element
        if i > 0 and a == nums[i-1]:
            continue
            
        l, r = i+1, len(nums)-1
        
        while l < r:
            threeSum = a + nums[l] + nums[r]
            
            if threeSum < 0:
                l += 1
            elif threeSum > 0:
                r -= 1
            else:
                res.append([a, nums[l], nums[r]])
                l += 1
                # Skip duplicate values for the second element
                while nums[l] == nums[l-1] and l < r:
                    l += 1
                    
    return res
```

## JavaScript Implementation

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
function threeSum(nums) {
    const res = [];
    nums.sort((a, b) => a - b);
    
    for (let i = 0; i < nums.length; i++) {
        // Skip duplicate values for the first element
        if (i > 0 && nums[i] === nums[i-1]) {
            continue;
        }
        
        let l = i + 1;
        let r = nums.length - 1;
        
        while (l < r) {
            const sum = nums[i] + nums[l] + nums[r];
            
            if (sum < 0) {
                l++;
            } else if (sum > 0) {
                r--;
            } else {
                res.push([nums[i], nums[l], nums[r]]);
                l++;
                // Skip duplicate values for the second element
                while (nums[l] === nums[l-1] && l < r) {
                    l++;
                }
            }
        }
    }
    
    return res;
}
```

## Java Implementation

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);
        
        for (int i = 0; i < nums.length; i++) {
            // Skip duplicate values for the first element
            if (i > 0 && nums[i] == nums[i-1]) {
                continue;
            }
            
            int l = i + 1;
            int r = nums.length - 1;
            
            while (l < r) {
                int sum = nums[i] + nums[l] + nums[r];
                
                if (sum < 0) {
                    l++;
                } else if (sum > 0) {
                    r--;
                } else {
                    res.add(Arrays.asList(nums[i], nums[l], nums[r]));
                    l++;
                    // Skip duplicate values for the second element
                    while (l < r && nums[l] == nums[l-1]) {
                        l++;
                    }
                }
            }
        }
        
        return res;
    }
}
```
