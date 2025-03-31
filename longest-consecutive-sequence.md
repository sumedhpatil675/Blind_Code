# Longest Consecutive Sequence

## Problem Description
Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

## Approaches

### 1. Brute Force
* Time complexity: O(n²)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        if not nums:
            return 0
            
        res = 0
        store = set(nums)
        
        for num in nums:
            streak, curr = 0, num
            while curr in store:
                streak += 1
                curr += 1
            res = max(res, streak)
            
        return res
```

#### JavaScript
```javascript
var longestConsecutive = function(nums) {
    if (nums.length === 0) {
        return 0;
    }
    
    const numSet = new Set(nums);
    let result = 0;
    
    for (const num of nums) {
        let streak = 0;
        let current = num;
        
        while (numSet.has(current)) {
            streak++;
            current++;
        }
        
        result = Math.max(result, streak);
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        
        Set<Integer> numSet = new HashSet<>();
        for (int num : nums) {
            numSet.add(num);
        }
        
        int result = 0;
        
        for (int num : nums) {
            int streak = 0;
            int current = num;
            
            while (numSet.contains(current)) {
                streak++;
                current++;
            }
            
            result = Math.max(result, streak);
        }
        
        return result;
    }
}
```

### 2. Sorting
* Time complexity: O(n log n)
* Space complexity: O(1) or O(n) depending on sort implementation

#### Python
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        if not nums:
            return 0
            
        nums.sort()
        
        res = 0
        curr, streak = nums[0], 1
        
        for i in range(1, len(nums)):
            if nums[i] == nums[i-1]:
                continue
                
            if nums[i] == curr + 1:
                streak += 1
            else:
                res = max(res, streak)
                streak = 1
                
            curr = nums[i]
            
        return max(res, streak)
```

#### JavaScript
```javascript
var longestConsecutive = function(nums) {
    if (nums.length === 0) {
        return 0;
    }
    
    nums.sort((a, b) => a - b);
    
    let result = 1;
    let currentStreak = 1;
    
    for (let i = 1; i < nums.length; i++) {
        // Skip duplicates
        if (nums[i] === nums[i-1]) {
            continue;
        }
        
        if (nums[i] === nums[i-1] + 1) {
            currentStreak++;
        } else {
            result = Math.max(result, currentStreak);
            currentStreak = 1;
        }
    }
    
    return Math.max(result, currentStreak);
};
```

#### Java
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        
        Arrays.sort(nums);
        
        int result = 1;
        int currentStreak = 1;
        
        for (int i = 1; i < nums.length; i++) {
            // Skip duplicates
            if (nums[i] == nums[i-1]) {
                continue;
            }
            
            if (nums[i] == nums[i-1] + 1) {
                currentStreak++;
            } else {
                result = Math.max(result, currentStreak);
                currentStreak = 1;
            }
        }
        
        return Math.max(result, currentStreak);
    }
}
```

### 3. Hash Set (Optimal)
* Time complexity: O(n)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        numSet = set(nums)
        longest = 0
        
        for num in numSet:
            # Only start a sequence from its beginning
            if (num - 1) not in numSet:
                length = 1
                while (num + length) in numSet:
                    length += 1
                    
                longest = max(length, longest)
                
        return longest
```

#### JavaScript
```javascript
var longestConsecutive = function(nums) {
    const numSet = new Set(nums);
    let longest = 0;
    
    for (const num of numSet) {
        // Only start a sequence from its beginning
        if (!numSet.has(num - 1)) {
            let length = 1;
            
            while (numSet.has(num + length)) {
                length++;
            }
            
            longest = Math.max(longest, length);
        }
    }
    
    return longest;
};
```

#### Java
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> numSet = new HashSet<>();
        for (int num : nums) {
            numSet.add(num);
        }
        
        int longest = 0;
        
        for (int num : numSet) {
            // Only start a sequence from its beginning
            if (!numSet.contains(num - 1)) {
                int length = 1;
                
                while (numSet.contains(num + length)) {
                    length++;
                }
                
                longest = Math.max(longest, length);
            }
        }
        
        return longest;
    }
}
```

### 4. Hash Map (Advanced)
* Time complexity: O(n) amortized
* Space complexity: O(n)

#### Python
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        mp = defaultdict(int)
        res = 0
        
        for num in nums:
            if not mp[num]:
                mp[num] = mp[num - 1] + mp[num + 1] + 1
                mp[num - mp[num - 1]] = mp[num]
                mp[num + mp[num + 1]] = mp[num]
                res = max(res, mp[num])
                
        return res
```

#### JavaScript
```javascript
var longestConsecutive = function(nums) {
    const map = new Map();
    let result = 0;
    
    for (const num of nums) {
        if (!map.has(num)) {
            const left = map.get(num - 1) || 0;
            const right = map.get(num + 1) || 0;
            
            const length = left + right + 1;
            
            // Update length at current position
            map.set(num, length);
            
            // Update length at boundaries
            map.set(num - left, length);
            map.set(num + right, length);
            
            result = Math.max(result, length);
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int result = 0;
        
        for (int num : nums) {
            if (!map.containsKey(num)) {
                int left = map.getOrDefault(num - 1, 0);
                int right = map.getOrDefault(num + 1, 0);
                
                int length = left + right + 1;
                
                // Update length at current position
                map.put(num, length);
                
                // Update length at boundaries
                map.put(num - left, length);
                map.put(num + right, length);
                
                result = Math.max(result, length);
            }
        }
        
        return result;
    }
}
```