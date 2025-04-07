# Group Anagrams

## Problem Description
Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

## Approaches

### 1. Sorting
* Time complexity: O(m * n log n) where m is the number of strings and n is the length of the longest string
* Space complexity: O(m * n)

#### Python
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        res = defaultdict(list)
        
        for s in strs:
            sortedS = ''.join(sorted(s))
            res[sortedS].append(s)
            
        return list(res.values())
```

#### JavaScript
```javascript
var groupAnagrams = function(strs) {
    const map = new Map();
    
    for (const str of strs) {
        const sortedStr = str.split('').sort().join('');
        
        if (!map.has(sortedStr)) {
            map.set(sortedStr, []);
        }
        
        map.get(sortedStr).push(str);
    }
    
    return Array.from(map.values());
};
```

#### Java
```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        
        for (String str : strs) {
            char[] chars = str.toCharArray();
            Arrays.sort(chars);
            String sortedStr = new String(chars);
            
            if (!map.containsKey(sortedStr)) {
                map.put(sortedStr, new ArrayList<>());
            }
            
            map.get(sortedStr).add(str);
        }
        
        return new ArrayList<>(map.values());
    }
}
```

### 2. Character Count ✅
* Time complexity: O(m * n) where m is the number of strings and n is the length of the longest string
* Space complexity: O(m * n)

#### Python
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        res = defaultdict(list)
        
        for s in strs:
            count = [0] * 26
            
            for c in s:
                count[ord(c) - ord('a')] += 1
                
            res[tuple(count)].append(s)
            
        return list(res.values())
```

#### JavaScript
```javascript
var groupAnagrams = function(strs) {
    const map = new Map();
    
    for (const str of strs) {
        // Create a count array for 26 lowercase letters
        const count = Array(26).fill(0);
        
        // Count each character
        for (const char of str) {
            const idx = char.charCodeAt(0) - 'a'.charCodeAt(0);
            count[idx]++;
        }
        
        // Use the count array as key (convert to string to use as a map key)
        const key = count.toString();
        
        if (!map.has(key)) {
            map.set(key, []);
        }
        
        map.get(key).push(str);
    }
    
    return Array.from(map.values());
};
```

#### Java
```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        
        for (String str : strs) {
            // Create a count array for 26 lowercase letters
            int[] count = new int[26];
            
            // Count each character
            for (char c : str.toCharArray()) {
                count[c - 'a']++;
            }
            
            // Convert count array to a string key
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < 26; i++) {
                sb.append('#');
                sb.append(count[i]);
            }
            String key = sb.toString();
            
            if (!map.containsKey(key)) {
                map.put(key, new ArrayList<>());
            }
            
            map.get(key).add(str);
        }
        
        return new ArrayList<>(map.values());
    }
}
```
