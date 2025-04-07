# Encode and Decode Strings

Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.

## Approaches

### 1. Length-Based Encoding ✅
- **Time complexity**: O(n) for both encode and decode
- **Space complexity**: O(n) for both encode and decode

Where n is the total length of all strings.

#### Python
```python
def encode(strs):
    result = ""
    for s in strs:
        result += str(len(s)) + "#" + s
    return result

def decode(s):
    result = []
    i = 0
    while i < len(s):
        # Find the delimiter '#'
        j = i
        while s[j] != '#':
            j += 1
        
        # Extract the length
        length = int(s[i:j])
        
        # Extract the string
        i = j + 1
        j = i + length
        result.append(s[i:j])
        
        # Move to the next string
        i = j
    
    return result
```

#### JavaScript
```javascript
function encode(strs) {
    let result = "";
    for (const s of strs) {
        result += s.length + "#" + s;
    }
    return result;
}

function decode(s) {
    const result = [];
    let i = 0;
    
    while (i < s.length) {
        // Find the delimiter '#'
        let j = i;
        while (s[j] !== '#') {
            j++;
        }
        
        // Extract the length
        const length = parseInt(s.substring(i, j));
        
        // Extract the string
        i = j + 1;
        j = i + length;
        result.push(s.substring(i, j));
        
        // Move to the next string
        i = j;
    }
    
    return result;
}
```

#### Java
```java
public String encode(List<String> strs) {
    StringBuilder sb = new StringBuilder();
    for (String s : strs) {
        sb.append(s.length()).append("#").append(s);
    }
    return sb.toString();
}

public List<String> decode(String s) {
    List<String> result = new ArrayList<>();
    int i = 0;
    
    while (i < s.length()) {
        // Find the delimiter '#'
        int j = i;
        while (s.charAt(j) != '#') {
            j++;
        }
        
        // Extract the length
        int length = Integer.parseInt(s.substring(i, j));
        
        // Extract the string
        i = j + 1;
        j = i + length;
        result.add(s.substring(i, j));
        
        // Move to the next string
        i = j;
    }
    
    return result;
}
```

### 2. Delimiter-Based Encoding
- **Time complexity**: O(n) for both encode and decode
- **Space complexity**: O(n) for both encode and decode

This approach uses a non-ASCII character as a delimiter.

#### Python
```python
def encode(strs):
    # Using a non-ASCII delimiter that won't appear in the input
    return chr(257).join(strs)

def decode(s):
    # Split on the non-ASCII delimiter
    return s.split(chr(257))
```

#### JavaScript
```javascript
function encode(strs) {
    // Using a non-ASCII delimiter that won't appear in the input
    return strs.join(String.fromCharCode(257));
}

function decode(s) {
    // Split on the non-ASCII delimiter
    return s.split(String.fromCharCode(257));
}
```

#### Java
```java
public String encode(List<String> strs) {
    // Using a non-ASCII delimiter that won't appear in the input
    return String.join(String.valueOf((char)257), strs);
}

public List<String> decode(String s) {
    // Split on the non-ASCII delimiter
    return Arrays.asList(s.split(String.valueOf((char)257), -1));
}
```

### 3. Chunk-Based Encoding
- **Time complexity**: O(n) for both encode and decode
- **Space complexity**: O(n) for both encode and decode

#### Python
```python
def encode(strs):
    result = ""
    for s in strs:
        sizes, chunk = [], ""
        for c in str(len(s)):
            sizes.append(c)
        
        result += "".join(sizes) + ","
        result += s
    
    return result

def decode(s):
    result = []
    i = 0
    
    while i < len(s):
        # Find the delimiter ','
        j = i
        while s[j] != ',':
            j += 1
        
        # Extract the length
        length = int(s[i:j])
        
        # Extract the string
        i = j + 1
        j = i + length
        result.append(s[i:j])
        
        # Move to the next string
        i = j
    
    return result
```

#### JavaScript
```javascript
function encode(strs) {
    let result = "";
    for (const s of strs) {
        result += s.length + "," + s;
    }
    return result;
}

function decode(s) {
    const result = [];
    let i = 0;
    
    while (i < s.length) {
        // Find the delimiter ','
        let j = i;
        while (s[j] !== ',') {
            j++;
        }
        
        // Extract the length
        const length = parseInt(s.substring(i, j));
        
        // Extract the string
        i = j + 1;
        j = i + length;
        result.push(s.substring(i, j));
        
        // Move to the next string
        i = j;
    }
    
    return result;
}
```

#### Java
```java
public String encode(List<String> strs) {
    StringBuilder sb = new StringBuilder();
    for (String s : strs) {
        sb.append(s.length()).append(",").append(s);
    }
    return sb.toString();
}

public List<String> decode(String s) {
    List<String> result = new ArrayList<>();
    int i = 0;
    
    while (i < s.length()) {
        // Find the delimiter ','
        int j = i;
        while (s.charAt(j) != ',') {
            j++;
        }
        
        // Extract the length
        int length = Integer.parseInt(s.substring(i, j));
        
        // Extract the string
        i = j + 1;
        j = i + length;
        result.add(s.substring(i, j));
        
        // Move to the next string
        i = j;
    }
    
    return result;
}
```
