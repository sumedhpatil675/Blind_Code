# Minimum Window Substring

## Problem Description
Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.

The testcases will be generated such that the answer is unique.

## Approaches

### 1. Brute Force
* Time complexity: O(n³) where n is the length of string s
* Space complexity: O(m + n) where m is the length of string t

#### Python
```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if t == "":
            return ""
            
        countT = {}
        for c in t:
            countT[c] = 1 + countT.get(c, 0)
            
        res, resLen = [-1, -1], float("infinity")
        
        for i in range(len(s)):
            for j in range(i, len(s)):
                # Check if s[i:j+1] contains all characters in t
                window = s[i:j+1]
                countS = {}
                for c in window:
                    countS[c] = 1 + countS.get(c, 0)
                
                valid = True
                for c in countT:
                    if countT[c] > countS.get(c, 0):
                        valid = False
                        break
                
                if valid and (j - i + 1) < resLen:
                    res = [i, j]
                    resLen = j - i + 1
        
        l, r = res
        return s[l:r+1] if resLen != float("infinity") else ""
```

#### JavaScript
```javascript
var minWindow = function(s, t) {
    if (t === "") {
        return "";
    }
    
    const countT = {};
    for (const c of t) {
        countT[c] = (countT[c] || 0) + 1;
    }
    
    let result = [-1, -1];
    let minLength = Infinity;
    
    for (let i = 0; i < s.length; i++) {
        for (let j = i; j < s.length; j++) {
            // Check if s.substring(i, j+1) contains all characters in t
            const countS = {};
            for (let k = i; k <= j; k++) {
                const c = s[k];
                countS[c] = (countS[c] || 0) + 1;
            }
            
            let valid = true;
            for (const c in countT) {
                if (!countS[c] || countS[c] < countT[c]) {
                    valid = false;
                    break;
                }
            }
            
            if (valid && (j - i + 1) < minLength) {
                result = [i, j];
                minLength = j - i + 1;
            }
        }
    }
    
    return minLength === Infinity ? "" : s.substring(result[0], result[1] + 1);
};
```

#### Java
```java
class Solution {
    public String minWindow(String s, String t) {
        if (t.isEmpty()) {
            return "";
        }
        
        Map<Character, Integer> countT = new HashMap<>();
        for (char c : t.toCharArray()) {
            countT.put(c, countT.getOrDefault(c, 0) + 1);
        }
        
        int[] result = {-1, -1};
        int minLength = Integer.MAX_VALUE;
        
        for (int i = 0; i < s.length(); i++) {
            for (int j = i; j < s.length(); j++) {
                // Check if s.substring(i, j+1) contains all characters in t
                Map<Character, Integer> countS = new HashMap<>();
                for (int k = i; k <= j; k++) {
                    char c = s.charAt(k);
                    countS.put(c, countS.getOrDefault(c, 0) + 1);
                }
                
                boolean valid = true;
                for (char c : countT.keySet()) {
                    if (!countS.containsKey(c) || countS.get(c) < countT.get(c)) {
                        valid = false;
                        break;
                    }
                }
                
                if (valid && (j - i + 1) < minLength) {
                    result = new int[]{i, j};
                    minLength = j - i + 1;
                }
            }
        }
        
        return minLength == Integer.MAX_VALUE ? "" : s.substring(result[0], result[1] + 1);
    }
}
```

### 2. Sliding Window (Optimal)
* Time complexity: O(n + m) where n is the length of string s and m is the length of string t
* Space complexity: O(n + m)

#### Python
```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if t == "":
            return ""
            
        # Character frequency map for t
        countT = {}
        for c in t:
            countT[c] = 1 + countT.get(c, 0)
            
        # Window character frequency map
        window = {}
        have, need = 0, len(countT)  # Number of fulfilled character requirements
        
        res, resLen = [-1, -1], float("infinity")
        l = 0
        
        for r in range(len(s)):
            c = s[r]
            window[c] = 1 + window.get(c, 0)
            
            # If we've matched the required count for a character
            if c in countT and window[c] == countT[c]:
                have += 1
                
            # Try to minimize the window
            while have == need:
                # Update our result
                if (r - l + 1) < resLen:
                    res = [l, r]
                    resLen = r - l + 1
                    
                # Remove left character from window
                window[s[l]] -= 1
                
                # If we remove a necessary character, we need to look for it again
                if s[l] in countT and window[s[l]] < countT[s[l]]:
                    have -= 1
                    
                l += 1
                
        l, r = res
        return s[l:r+1] if resLen != float("infinity") else ""
```

#### JavaScript
```javascript
var minWindow = function(s, t) {
    if (t === "") {
        return "";
    }
    
    // Character frequency map for t
    const countT = {};
    for (const c of t) {
        countT[c] = (countT[c] || 0) + 1;
    }
    
    // Window character frequency map
    const window = {};
    let have = 0;
    const need = Object.keys(countT).length;  // Number of unique characters needed
    
    let result = [-1, -1];
    let minLength = Infinity;
    let left = 0;
    
    for (let right = 0; right < s.length; right++) {
        const c = s[right];
        window[c] = (window[c] || 0) + 1;
        
        // If we've matched the required count for a character
        if (countT[c] && window[c] === countT[c]) {
            have++;
        }
        
        // Try to minimize the window
        while (have === need) {
            // Update our result
            if (right - left + 1 < minLength) {
                result = [left, right];
                minLength = right - left + 1;
            }
            
            // Remove left character from window
            const leftChar = s[left];
            window[leftChar]--;
            
            // If we remove a necessary character, we need to look for it again
            if (countT[leftChar] && window[leftChar] < countT[leftChar]) {
                have--;
            }
            
            left++;
        }
    }
    
    return minLength === Infinity ? "" : s.substring(result[0], result[1] + 1);
};
```

#### Java
```java
class Solution {
    public String minWindow(String s, String t) {
        if (t.isEmpty()) {
            return "";
        }
        
        // Character frequency map for t
        Map<Character, Integer> countT = new HashMap<>();
        for (char c : t.toCharArray()) {
            countT.put(c, countT.getOrDefault(c, 0) + 1);
        }
        
        // Window character frequency map
        Map<Character, Integer> window = new HashMap<>();
        int have = 0;
        int need = countT.size();  // Number of unique characters needed
        
        int[] result = {-1, -1};
        int minLength = Integer.MAX_VALUE;
        int left = 0;
        
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            window.put(c, window.getOrDefault(c, 0) + 1);
            
            // If we've matched the required count for a character
            if (countT.containsKey(c) && window.get(c).equals(countT.get(c))) {
                have++;
            }
            
            // Try to minimize the window
            while (have == need) {
                // Update our result
                if (right - left + 1 < minLength) {
                    result = new int[]{left, right};
                    minLength = right - left + 1;
                }
                
                // Remove left character from window
                char leftChar = s.charAt(left);
                window.put(leftChar, window.get(leftChar) - 1);
                
                // If we remove a necessary character, we need to look for it again
                if (countT.containsKey(leftChar) && window.get(leftChar) < countT.get(leftChar)) {
                    have--;
                }
                
                left++;
            }
        }
        
        return minLength == Integer.MAX_VALUE ? "" : s.substring(result[0], result[1] + 1);
    }
}
```