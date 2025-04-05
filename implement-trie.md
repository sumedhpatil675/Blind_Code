# Implement Trie (Prefix Tree)

A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:
- `Trie()` Initializes the trie object.
- `void insert(String word)` Inserts the string word into the trie.
- `boolean search(String word)` Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
- `boolean startsWith(String prefix)` Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.

## Approaches

### 1. Prefix Tree (Array)

**Time complexity:** O(n) for each function call.  
**Space complexity:** O(t)  
where n is the length of the string and t is the total number of TrieNodes created in the Trie.

#### Python
```python
class TrieNode:
    def __init__(self):
        self.children = [None] * 26
        self.endOfWord = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
        
    def insert(self, word: str) -> None:
        cur = self.root
        
        for c in word:
            i = ord(c) - ord("a")
            if cur.children[i] == None:
                cur.children[i] = TrieNode()
            cur = cur.children[i]
            
        cur.endOfWord = True
        
    def search(self, word: str) -> bool:
        cur = self.root
        
        for c in word:
            i = ord(c) - ord("a")
            if cur.children[i] == None:
                return False
            cur = cur.children[i]
            
        return cur.endOfWord
        
    def startsWith(self, prefix: str) -> bool:
        cur = self.root
        
        for c in prefix:
            i = ord(c) - ord("a")
            if cur.children[i] == None:
                return False
            cur = cur.children[i]
            
        return True
```

# Trie Data Structure Explanation

The code implements a Trie (prefix tree) data structure that efficiently stores and retrieves strings. Let's break down how this works with ASCII diagrams.

## Trie Components

1. **TrieNode**: Each node contains:
   - An array of 26 child pointers (one for each lowercase letter)
   - A boolean flag indicating if the node represents the end of a word

2. **Trie**: The main class that manages operations on the structure:
   - `insert(word)`: Adds a new word to the trie
   - `search(word)`: Checks if a complete word exists
   - `startsWith(prefix)`: Checks if any word with the given prefix exists

## ASCII Diagram Illustrations

### Example: Building a Trie with words "app", "apple", and "api"

Let's visualize the insertion process step by step:

#### Initially (empty trie):
```
Root
│
└── [a-z] (all None)
```

#### After inserting "app":

```
Root
│
└── a ────────────────┐
    [b-z] (all None)  │
                       │
                       p ────────────────┐
                       [a-o,q-z] (None)  │
                                         │
                                         p ────────────────┐
                                         [a-z] (all None)  │
                                                           │
                                                           # (endOfWord = True)
```

#### After inserting "apple":

```
Root
│
└── a ────────────────┐
    [b-z] (all None)  │
                       │
                       p ────────────────┐
                       [a-o,q-z] (None)  │
                                         │
                                         p ────────────────┐
                                         [a-z] (all None)  │
                                         │                 │
                                         │                 # (endOfWord = True)
                                         │
                                         l ────────────────┐
                                         [a-d,f-z] (None)  │
                                                           │
                                                           e ────────────────┐
                                                           [a-z] (all None)  │
                                                                             │
                                                                             # (endOfWord = True)
```

#### After inserting "api":

```
Root
│
└── a ────────────────┐
    [b-z] (all None)  │
                       │
                       p ────────────────┐
                       [a-o,q-z] (None)  │
                       │                 │
                       │                 p ────────────────┐
                       │                 [a-z] (all None)  │
                       │                 │                 │
                       │                 │                 # (endOfWord = True)
                       │                 │
                       │                 l ────────────────┐
                       │                 [a-d,f-z] (None)  │
                       │                                   │
                       │                                   e ────────────────┐
                       │                                   [a-z] (all None)  │
                       │                                                     │
                       │                                                     # (endOfWord = True)
                       │
                       i ────────────────┐
                       [a-z] (all None)  │
                                         │
                                         # (endOfWord = True)

### Search Example: Looking for "app"

1. Start at Root, follow child[0] ('a') → Node 1
2. At Node 1, follow child[15] ('p') → Node 2
3. At Node 2, follow child[15] ('p') → Node 3
4. At Node 3, check endOfWord == True? Yes, return True

### StartsWith Example: Checking for prefix "ap"

1. Start at Root, follow child[0] ('a') → Node 1
2. At Node 1, follow child[15] ('p') → Node 2
3. Node 2 exists, so return True (doesn't matter if it's an end of word)

## Implementation Details

- The code uses an array of size 26 for each node, assuming only lowercase English letters.
- Character mapping: `index = ord(c) - ord('a')` converts 'a'→0, 'b'→1, etc.
- Space efficiency: O(n × m) where n is number of words and m is average word length.
- Time efficiency: O(m) for all operations where m is word length.

This implementation is memory-efficient for lowercase English alphabet but could be modified to support different character sets by changing the children array structure.


#### JavaScript

```javascript
class TrieNode {
    constructor() {
        this.children = Array(26).fill(null);
        this.endOfWord = false;
    }
}

/**
 * Initialize your data structure here.
 */
var Trie = function() {
    this.root = new TrieNode();
};

/**
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function(word) {
    let curr = this.root;
    
    for (let i = 0; i < word.length; i++) {
        const index = word.charCodeAt(i) - 'a'.charCodeAt(0);
        
        if (curr.children[index] === null) {
            curr.children[index] = new TrieNode();
        }
        
        curr = curr.children[index];
    }
    
    curr.endOfWord = true;
};

/**
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function(word) {
    let curr = this.root;
    
    for (let i = 0; i < word.length; i++) {
        const index = word.charCodeAt(i) - 'a'.charCodeAt(0);
        
        if (curr.children[index] === null) {
            return false;
        }
        
        curr = curr.children[index];
    }
    
    return curr.endOfWord;
};

/**
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function(prefix) {
    let curr = this.root;
    
    for (let i = 0; i < prefix.length; i++) {
        const index = prefix.charCodeAt(i) - 'a'.charCodeAt(0);
        
        if (curr.children[index] === null) {
            return false;
        }
        
        curr = curr.children[index];
    }
    
    return true;
};
```

#### Java
```java
class TrieNode {
    TrieNode[] children;
    boolean endOfWord;
    
    public TrieNode() {
        children = new TrieNode[26];
        endOfWord = false;
    }
}

class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode current = root;
        
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            
            if (current.children[index] == null) {
                current.children[index] = new TrieNode();
            }
            
            current = current.children[index];
        }
        
        current.endOfWord = true;
    }
    
    public boolean search(String word) {
        TrieNode current = root;
        
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            
            if (current.children[index] == null) {
                return false;
            }
            
            current = current.children[index];
        }
        
        return current.endOfWord;
    }
    
    public boolean startsWith(String prefix) {
        TrieNode current = root;
        
        for (char c : prefix.toCharArray()) {
            int index = c - 'a';
            
            if (current.children[index] == null) {
                return false;
            }
            
            current = current.children[index];
        }
        
        return true;
    }
}
```

### 2. Prefix Tree (Hash Map)

**Time complexity:** O(n) for each function call.  
**Space complexity:** O(t)  
where n is the length of the string and t is the total number of TrieNodes created in the Trie.

#### Python
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.endOfWord = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
        
    def insert(self, word: str) -> None:
        cur = self.root
        
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()
            cur = cur.children[c]
            
        cur.endOfWord = True
        
    def search(self, word: str) -> bool:
        cur = self.root
        
        for c in word:
            if c not in cur.children:
                return False
            cur = cur.children[c]
            
        return cur.endOfWord
        
    def startsWith(self, prefix: str) -> bool:
        cur = self.root
        
        for c in prefix:
            if c not in cur.children:
                return False
            cur = cur.children[c]
            
        return True
```

#### JavaScript
```javascript
class TrieNode {
    constructor() {
        this.children = {};
        this.endOfWord = false;
    }
}

/**
 * Initialize your data structure here.
 */
var Trie = function() {
    this.root = new TrieNode();
};

/**
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function(word) {
    let curr = this.root;
    
    for (let i = 0; i < word.length; i++) {
        const char = word[i];
        
        if (!(char in curr.children)) {
            curr.children[char] = new TrieNode();
        }
        
        curr = curr.children[char];
    }
    
    curr.endOfWord = true;
};

/**
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function(word) {
    let curr = this.root;
    
    for (let i = 0; i < word.length; i++) {
        const char = word[i];
        
        if (!(char in curr.children)) {
            return false;
        }
        
        curr = curr.children[char];
    }
    
    return curr.endOfWord;
};

/**
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function(prefix) {
    let curr = this.root;
    
    for (let i = 0; i < prefix.length; i++) {
        const char = prefix[i];
        
        if (!(char in curr.children)) {
            return false;
        }
        
        curr = curr.children[char];
    }
    
    return true;
};
```

#### Java
```java
class TrieNode {
    Map<Character, TrieNode> children;
    boolean endOfWord;
    
    public TrieNode() {
        children = new HashMap<>();
        endOfWord = false;
    }
}

class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode current = root;
        
        for (char c : word.toCharArray()) {
            current.children.putIfAbsent(c, new TrieNode());
            current = current.children.get(c);
        }
        
        current.endOfWord = true;
    }
    
    public boolean search(String word) {
        TrieNode current = root;
        
        for (char c : word.toCharArray()) {
            if (!current.children.containsKey(c)) {
                return false;
            }
            
            current = current.children.get(c);
        }
        
        return current.endOfWord;
    }
    
    public boolean startsWith(String prefix) {
        TrieNode current = root;
        
        for (char c : prefix.toCharArray()) {
            if (!current.children.containsKey(c)) {
                return false;
            }
            
            current = current.children.get(c);
        }
        
        return true;
    }
}
```
