# Valid Parentheses

## Problem Description
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

## Approaches

### 1. Brute Force (String Replacement)
* Time complexity: O(n²)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def isValid(self, s: str) -> bool:
        # Repeatedly replace matching pairs until none are left
        while '()' in s or '{}' in s or '[]' in s:
            s = s.replace('()', '')
            s = s.replace('{}', '')
            s = s.replace('[]', '')
            
        # If the string is empty, all brackets were matched
        return s == ''
```

#### JavaScript
```javascript
var isValid = function(s) {
    // Repeatedly replace matching pairs until none are left
    while (s.includes('()') || s.includes('{}') || s.includes('[]')) {
        s = s.replace('()', '');
        s = s.replace('{}', '');
        s = s.replace('[]', '');
    }
    
    // If the string is empty, all brackets were matched
    return s === '';
};
```

#### Java
```java
class Solution {
    public boolean isValid(String s) {
        // Repeatedly replace matching pairs until none are left
        while (s.contains("()") || s.contains("{}") || s.contains("[]")) {
            s = s.replace("()", "");
            s = s.replace("{}", "");
            s = s.replace("[]", "");
        }
        
        // If the string is empty, all brackets were matched
        return s.isEmpty();
    }
}
```

### 2. Stack ✅✅✅

* Time complexity: O(n)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        closeToOpen = {")": "(", "]": "[", "}": "{"}
        
        for c in s:
            # If it's a closing bracket
            if c in closeToOpen:
                # Check if stack is non-empty and the top matches the expected opening bracket
                if stack and stack[-1] == closeToOpen[c]:
                    stack.pop()
                else:
                    return False
            # If it's an opening bracket
            else:
                stack.append(c)
                
        # Stack should be empty if all brackets are properly matched
        return True if not stack else False
```

#### JavaScript
```javascript
var isValid = function(s) {
    const stack = [];
    const closeToOpen = {
        ')': '(',
        ']': '[',
        '}': '{'
    };
    
    for (const c of s) {
        // If it's a closing bracket
        if (c in closeToOpen) {
            // Check if stack is non-empty and the top matches the expected opening bracket
            if (stack.length > 0 && stack[stack.length - 1] === closeToOpen[c]) {
                stack.pop();
            } else {
                return false;
            }
        }
        // If it's an opening bracket
        else {
            stack.push(c);
        }
    }
    
    // Stack should be empty if all brackets are properly matched
    return stack.length === 0;
};
```

#### Java
```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        Map<Character, Character> closeToOpen = new HashMap<>();
        closeToOpen.put(')', '(');
        closeToOpen.put(']', '[');
        closeToOpen.put('}', '{');
        
        for (char c : s.toCharArray()) {
            // If it's a closing bracket
            if (closeToOpen.containsKey(c)) {
                // Check if stack is non-empty and the top matches the expected opening bracket
                if (!stack.isEmpty() && stack.peek().equals(closeToOpen.get(c))) {
                    stack.pop();
                } else {
                    return false;
                }
            }
            // If it's an opening bracket
            else {
                stack.push(c);
            }
        }
        
        // Stack should be empty if all brackets are properly matched
        return stack.isEmpty();
    }
}
```
