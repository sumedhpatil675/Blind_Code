# Valid Palindrome

## Problem Description
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

## Approaches

### 1. Clean String and Compare with Reverse
* Time complexity: O(n)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Create a clean string with only alphanumeric characters
        newStr = ''
        for c in s:
            if c.isalnum():
                newStr += c.lower()
                
        # Compare with its reverse
        return newStr == newStr[::-1]
```

#### JavaScript
```javascript
var isPalindrome = function(s) {
    // Create a clean string with only alphanumeric characters
    const cleanStr = s.toLowerCase().replace(/[^a-z0-9]/g, '');
    
    // Compare with its reverse
    const reversedStr = cleanStr.split('').reverse().join('');
    
    return cleanStr === reversedStr;
};
```

#### Java
```java
class Solution {
    public boolean isPalindrome(String s) {
        // Create a clean string with only alphanumeric characters
        StringBuilder cleanStr = new StringBuilder();
        
        for (char c : s.toCharArray()) {
            if (Character.isLetterOrDigit(c)) {
                cleanStr.append(Character.toLowerCase(c));
            }
        }
        
        // Compare with its reverse
        String forward = cleanStr.toString();
        String backward = cleanStr.reverse().toString();
        
        return forward.equals(backward);
    }
}
```

### 2. Two Pointers ✅✅✅

* Time complexity: O(n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l, r = 0, len(s) - 1
        
        while l < r:
            # Skip non-alphanumeric characters
            while l < r and not self.alphaNum(s[l]):
                l += 1
            while r > l and not self.alphaNum(s[r]):
                r -= 1
                
            # Compare characters
            if s[l].lower() != s[r].lower():
                return False
                
            l, r = l + 1, r - 1
            
        return True
    
    def alphaNum(self, c):
        return (ord('A') <= ord(c) <= ord('Z') or
                ord('a') <= ord(c) <= ord('z') or
                ord('0') <= ord(c) <= ord('9'))
```

#### JavaScript
```javascript
var isPalindrome = function(s) {
    let left = 0;
    let right = s.length - 1;
    
    while (left < right) {
        // Skip non-alphanumeric characters
        while (left < right && !isAlphaNum(s[left])) {
            left++;
        }
        while (right > left && !isAlphaNum(s[right])) {
            right--;
        }
        
        // Compare characters
        if (s[left].toLowerCase() !== s[right].toLowerCase()) {
            return false;
        }
        
        left++;
        right--;
    }
    
    return true;
    
    function isAlphaNum(char) {
        const code = char.charCodeAt(0);
        return (
            (code >= 48 && code <= 57) || // 0-9
            (code >= 65 && code <= 90) || // A-Z
            (code >= 97 && code <= 122)   // a-z
        );
    }
};
```

#### Java
```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        
        while (left < right) {
            // Skip non-alphanumeric characters
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }
            while (right > left && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }
            
            // Compare characters
            if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                return false;
            }
            
            left++;
            right--;
        }
        
        return true;
    }
}
```
