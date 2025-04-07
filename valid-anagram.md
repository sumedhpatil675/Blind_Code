# Valid Anagram

## Problem Description
Given two strings s and t, return true if t is an anagram of s, and false otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

## Approaches

### 1. Sorting
* Time complexity: O(n log n)
* Space complexity: O(n) depending on the sorting algorithm

#### Python
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        return sorted(s) == sorted(t)
```

#### JavaScript
```javascript
var isAnagram = function(s, t) {
    if (s.length !== t.length) return false;
    return s.split('').sort().join('') === t.split('').sort().join('');
};
```

#### Java
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        
        char[] sArray = s.toCharArray();
        char[] tArray = t.toCharArray();
        
        Arrays.sort(sArray);
        Arrays.sort(tArray);
        
        return Arrays.equals(sArray, tArray);
    }
}
```

### 2. Hash Table (Dictionary)
* Time complexity: O(n)
* Space complexity: O(1) - because there are only 26 lowercase letters

#### Python
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        
        countS, countT = {}, {}
        
        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)
            countT[t[i]] = 1 + countT.get(t[i], 0)
            
        return countS == countT
```

#### JavaScript
```javascript
var isAnagram = function(s, t) {
    if (s.length !== t.length) return false;
    
    const countS = {};
    const countT = {};
    
    for (let i = 0; i < s.length; i++) {
        countS[s[i]] = (countS[s[i]] || 0) + 1;
        countT[t[i]] = (countT[t[i]] || 0) + 1;
    }
    
    for (let char in countS) {
        if (countS[char] !== countT[char]) {
            return false;
        }
    }
    
    return true;
};
```

#### Java
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        
        HashMap<Character, Integer> countS = new HashMap<>();
        HashMap<Character, Integer> countT = new HashMap<>();
        
        for (int i = 0; i < s.length(); i++) {
            countS.put(s.charAt(i), countS.getOrDefault(s.charAt(i), 0) + 1);
            countT.put(t.charAt(i), countT.getOrDefault(t.charAt(i), 0) + 1);
        }
        
        return countS.equals(countT);
    }
}
```

### 3. Array Counter (Optimal for lowercase letters) ✅✅✅

* Time complexity: O(n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
            
        count = [0] * 26
        
        for i in range(len(s)):
            count[ord(s[i]) - ord('a')] += 1
            count[ord(t[i]) - ord('a')] -= 1
            
        for val in count:
            if val != 0:
                return False
                
        return True
```

#### JavaScript
```javascript
var isAnagram = function(s, t) {
    if (s.length !== t.length) return false;
    
    const count = new Array(26).fill(0);
    const aCharCode = 'a'.charCodeAt(0);
    
    for (let i = 0; i < s.length; i++) {
        count[s.charCodeAt(i) - aCharCode]++;
        count[t.charCodeAt(i) - aCharCode]--;
    }
    
    for (let val of count) {
        if (val !== 0) {
            return false;
        }
    }
    
    return true;
};
```

#### Java
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        
        int[] count = new int[26];
        
        for (int i = 0; i < s.length(); i++) {
            count[s.charAt(i) - 'a']++;
            count[t.charAt(i) - 'a']--;
        }
        
        for (int val : count) {
            if (val != 0) {
                return false;
            }
        }
        
        return true;
    }
}
```
