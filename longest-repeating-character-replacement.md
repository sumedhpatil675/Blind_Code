# Longest Repeating Character Replacement

## Problem Description
You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

## Approaches

### 1. Brute Force
* Time complexity: O(n³)
* Space complexity: O(1) - because there are only 26 uppercase letters

#### Python
```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        res = 0
        
        for i in range(len(s)):
            count = {}
            maxf = 0
            
            for j in range(i, len(s)):
                count[s[j]] = 1 + count.get(s[j], 0)
                maxf = max(maxf, count[s[j]])
                
                # If the window size minus the most frequent character count
                # is less than or equal to k, we can have a valid substring
                if (j - i + 1) - maxf <= k:
                    res = max(res, j - i + 1)
                    
        return res
```

#### JavaScript
```javascript
var characterReplacement = function(s, k) {
    let maxLength = 0;
    
    for (let i = 0; i < s.length; i++) {
        const count = {};
        let maxFreq = 0;
        
        for (let j = i; j < s.length; j++) {
            count[s[j]] = (count[s[j]] || 0) + 1;
            maxFreq = Math.max(maxFreq, count[s[j]]);
            
            // If the window size minus the most frequent character count
            // is less than or equal to k, we can have a valid substring
            if ((j - i + 1) - maxFreq <= k) {
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }
    }
    
    return maxLength;
};
```

#### Java
```java
class Solution {
    public int characterReplacement(String s, int k) {
        int maxLength = 0;
        
        for (int i = 0; i < s.length(); i++) {
            Map<Character, Integer> count = new HashMap<>();
            int maxFreq = 0;
            
            for (int j = i; j < s.length(); j++) {
                char c = s.charAt(j);
                count.put(c, count.getOrDefault(c, 0) + 1);
                maxFreq = Math.max(maxFreq, count.get(c));
                
                // If the window size minus the most frequent character count
                // is less than or equal to k, we can have a valid substring
                if ((j - i + 1) - maxFreq <= k) {
                    maxLength = Math.max(maxLength, j - i + 1);
                }
            }
        }
        
        return maxLength;
    }
}
```

### 2. Sliding Window per Character
* Time complexity: O(26 * n) which is O(n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        res = 0
        charSet = set(s)
        
        # For each character, try to make the longest substring with that character
        for c in charSet:
            l = 0
            count = 0
            
            for r in range(len(s)):
                if s[r] == c:
                    count += 1
                    
                # While the number of replacements needed > k, shrink the window
                while (r - l + 1) - count > k:
                    if s[l] == c:
                        count -= 1
                    l += 1
                    
                res = max(res, r - l + 1)
                
        return res
```

#### JavaScript
```javascript
var characterReplacement = function(s, k) {
    let maxLength = 0;
    const charSet = new Set(s.split(''));
    
    // For each character, try to make the longest substring with that character
    for (const c of charSet) {
        let left = 0;
        let count = 0;
        
        for (let right = 0; right < s.length; right++) {
            if (s[right] === c) {
                count++;
            }
            
            // While the number of replacements needed > k, shrink the window
            while ((right - left + 1) - count > k) {
                if (s[left] === c) {
                    count--;
                }
                left++;
            }
            
            maxLength = Math.max(maxLength, right - left + 1);
        }
    }
    
    return maxLength;
};
```

#### Java
```java
class Solution {
    public int characterReplacement(String s, int k) {
        int maxLength = 0;
        Set<Character> charSet = new HashSet<>();
        
        // Create a set of unique characters in the string
        for (char c : s.toCharArray()) {
            charSet.add(c);
        }
        
        // For each character, try to make the longest substring with that character
        for (char c : charSet) {
            int left = 0;
            int count = 0;
            
            for (int right = 0; right < s.length(); right++) {
                if (s.charAt(right) == c) {
                    count++;
                }
                
                // While the number of replacements needed > k, shrink the window
                while ((right - left + 1) - count > k) {
                    if (s.charAt(left) == c) {
                        count--;
                    }
                    left++;
                }
                
                maxLength = Math.max(maxLength, right - left + 1);
            }
        }
        
        return maxLength;
    }
}
```

### 3. Sliding Window (Optimal) ✅✅✅
* Time complexity: O(n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        count = {}
        res = 0
        l = 0
        maxf = 0
        
        for r in range(len(s)):
            # Update character frequency
            count[s[r]] = 1 + count.get(s[r], 0)
            
            # Keep track of the most frequent character in the current window
            maxf = max(maxf, count[s[r]])
            
            # If the window size minus the most frequent character count
            # is greater than k, we need to shrink the window
            if (r - l + 1) - maxf > k:
                count[s[l]] -= 1
                l += 1
                
            res = max(res, r - l + 1)
            
        return res
```

#### JavaScript
```javascript
var characterReplacement = function(s, k) {
    const count = {};
    let maxLength = 0;
    let left = 0;
    let maxFreq = 0;
    
    for (let right = 0; right < s.length; right++) {
        // Update character frequency
        count[s[right]] = (count[s[right]] || 0) + 1;
        
        // Keep track of the most frequent character in the current window
        maxFreq = Math.max(maxFreq, count[s[right]]);
        
        // If the window size minus the most frequent character count
        // is greater than k, we need to shrink the window
        if ((right - left + 1) - maxFreq > k) {
            count[s[left]]--;
            left++;
        }
        
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
};
```

#### Java
```java
class Solution {
    public int characterReplacement(String s, int k) {
        Map<Character, Integer> count = new HashMap<>();
        int maxLength = 0;
        int left = 0;
        int maxFreq = 0;
        
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            
            // Update character frequency
            count.put(c, count.getOrDefault(c, 0) + 1);
            
            // Keep track of the most frequent character in the current window
            maxFreq = Math.max(maxFreq, count.get(c));
            
            // If the window size minus the most frequent character count
            // is greater than k, we need to shrink the window
            if ((right - left + 1) - maxFreq > k) {
                char leftChar = s.charAt(left);
                count.put(leftChar, count.get(leftChar) - 1);
                left++;
            }
            
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
}
```
