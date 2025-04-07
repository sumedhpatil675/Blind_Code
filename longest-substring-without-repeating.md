# Longest Substring Without Repeating Characters

## Problem Description
Given a string `s`, find the length of the longest substring without repeating characters.

## Approaches

### 1. Brute Force
* Time complexity: O(n³) where n is the length of the string
* Space complexity: O(min(n, m)) where m is the size of the character set

#### Python
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        res = 0
        
        for i in range(len(s)):
            charSet = set()
            for j in range(i, len(s)):
                if s[j] in charSet:
                    break
                charSet.add(s[j])
                res = max(res, len(charSet))
                
        return res
```

#### JavaScript
```javascript
var lengthOfLongestSubstring = function(s) {
    let maxLength = 0;
    
    for (let i = 0; i < s.length; i++) {
        const charSet = new Set();
        for (let j = i; j < s.length; j++) {
            if (charSet.has(s[j])) {
                break;
            }
            charSet.add(s[j]);
            maxLength = Math.max(maxLength, charSet.size);
        }
    }
    
    return maxLength;
};
```

#### Java
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int maxLength = 0;
        
        for (int i = 0; i < s.length(); i++) {
            Set<Character> charSet = new HashSet<>();
            for (int j = i; j < s.length(); j++) {
                if (charSet.contains(s.charAt(j))) {
                    break;
                }
                charSet.add(s.charAt(j));
                maxLength = Math.max(maxLength, charSet.size());
            }
        }
        
        return maxLength;
    }
}
```

### 2. Sliding Window with Set ✅✅✅

* Time complexity: O(n) where n is the length of the string
* Space complexity: O(min(n, m)) where m is the size of the character set

#### Python
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        charSet = set()
        l = 0
        res = 0
        
        for r in range(len(s)):
            # If we find a repeating character, remove characters from left until it's gone
            while s[r] in charSet:
                charSet.remove(s[l])
                l += 1
            
            charSet.add(s[r])
            res = max(res, r - l + 1)
            
        return res
```

#### JavaScript
```javascript
var lengthOfLongestSubstring = function(s) {
    const charSet = new Set();
    let left = 0;
    let maxLength = 0;
    
    for (let right = 0; right < s.length; right++) {
        // If we find a repeating character, remove characters from left until it's gone
        while (charSet.has(s[right])) {
            charSet.delete(s[left]);
            left++;
        }
        
        charSet.add(s[right]);
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
};
```

#### Java
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> charSet = new HashSet<>();
        int left = 0;
        int maxLength = 0;
        
        for (int right = 0; right < s.length(); right++) {
            // If we find a repeating character, remove characters from left until it's gone
            while (charSet.contains(s.charAt(right))) {
                charSet.remove(s.charAt(left));
                left++;
            }
            
            charSet.add(s.charAt(right));
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
}
```

### 3. Sliding Window with HashMap (Optimal)
* Time complexity: O(n) where n is the length of the string
* Space complexity: O(min(n, m)) where m is the size of the character set

#### Python
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        charMap = {}
        l = 0
        res = 0
        
        for r in range(len(s)):
            # If we find a repeating character, jump the left pointer to after the previous occurrence
            if s[r] in charMap:
                l = max(charMap[s[r]] + 1, l)
            
            charMap[s[r]] = r
            res = max(res, r - l + 1)
            
        return res
```

#### JavaScript
```javascript
var lengthOfLongestSubstring = function(s) {
    const charMap = new Map();
    let left = 0;
    let maxLength = 0;
    
    for (let right = 0; right < s.length; right++) {
        // If we find a repeating character, jump the left pointer to after the previous occurrence
        if (charMap.has(s[right])) {
            left = Math.max(charMap.get(s[right]) + 1, left);
        }
        
        charMap.set(s[right], right);
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
};
```

#### Java
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> charMap = new HashMap<>();
        int left = 0;
        int maxLength = 0;
        
        for (int right = 0; right < s.length(); right++) {
            char currentChar = s.charAt(right);
            
            // If we find a repeating character, jump the left pointer to after the previous occurrence
            if (charMap.containsKey(currentChar)) {
                left = Math.max(charMap.get(currentChar) + 1, left);
            }
            
            charMap.put(currentChar, right);
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
}
```
