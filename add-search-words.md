# Design Add and Search Words Data Structure

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the WordDictionary class:
- `WordDictionary()` - Initializes the object.
- `void addWord(word)` - Adds word to the data structure, it can be matched later.
- `bool search(word)` - Returns true if there is any string in the data structure that matches word or false otherwise. word may contain dots '.' where dots can be matched with any letter.

## Approaches

### 1. Brute Force

**Time complexity:**  
- `addWord()`: O(1)
- `search()`: O(m*n)  

**Space complexity:** O(m*n)  
where m is the number of words added and n is the length of the string.

#### Python
```python
class WordDictionary:
    def __init__(self):
        self.store = []
        
    def addWord(self, word: str) -> None:
        self.store.append(word)
        
    def search(self, word: str) -> bool:
        for w in self.store:
            if len(w) != len(word):
                continue
                
            i = 0
            while i < len(w):
                if w[i] == word[i] or word[i] == '.':
                    i += 1
                else:
                    break
                    
            if i == len(w):
                return True
                
        return False
```

#### JavaScript
```javascript
/**
 * Initialize your data structure here.
 */
var WordDictionary = function() {
    this.store = [];
};

/** 
 * @param {string} word
 * @return {void}
 */
WordDictionary.prototype.addWord = function(word) {
    this.store.push(word);
};

/** 
 * @param {string} word
 * @return {boolean}
 */
WordDictionary.prototype.search = function(word) {
    for (let w of this.store) {
        if (w.length !== word.length) {
            continue;
        }
        
        let i = 0;
        while (i < w.length) {
            if (w[i] === word[i] || word[i] === '.') {
                i++;
            } else {
                break;
            }
        }
        
        if (i === w.length) {
            return true;
        }
    }
    
    return false;
};
```

#### Java
```java
class WordDictionary {
    private List<String> store;

    /** Initialize your data structure here. */
    public WordDictionary() {
        store = new ArrayList<>();
    }
    
    public void addWord(String word) {
        store.add(word);
    }
    
    public boolean search(String word) {
        for (String w : store) {
            if (w.length() != word.length()) {
                continue;
            }
            
            int i = 0;
            while (i < w.length()) {
                if (w.charAt(i) == word.charAt(i) || word.charAt(i) == '.') {
                    i++;
                } else {
                    break;
                }
            }
            
            if (i == w.length()) {
                return true;
            }
        }
        
        return false;
    }
}
```

### 2. Depth First Search (Trie)

**Time complexity:**  
- `addWord()`: O(n)
- `search()`: O(n) for normal search, potentially O(26^m) for patterns with multiple '.' characters.  

**Space complexity:** O(t+n)  
where n is the length of the string and t is the total number of TrieNodes created in the Trie.

#### Python
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = False

class WordDictionary:
    def __init__(self):
        self.root = TrieNode()
        
    def addWord(self, word: str) -> None:
        cur = self.root
        
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()
            cur = cur.children[c]
            
        cur.word = True
        
    def search(self, word: str) -> bool:
        def dfs(j, root):
            cur = root
            
            for i in range(j, len(word)):
                c = word[i]
                
                if c == ".":
                    for child in cur.children.values():
                        if dfs(i + 1, child):
                            return True
                    return False
                else:
                    if c not in cur.children:
                        return False
                    cur = cur.children[c]
                    
            return cur.word
            
        return dfs(0, self.root)
```

#### JavaScript
```javascript
class TrieNode {
    constructor() {
        this.children = {};
        this.isWord = false;
    }
}

/**
 * Initialize your data structure here.
 */
var WordDictionary = function() {
    this.root = new TrieNode();
};

/** 
 * @param {string} word
 * @return {void}
 */
WordDictionary.prototype.addWord = function(word) {
    let curr = this.root;
    
    for (let c of word) {
        if (!(c in curr.children)) {
            curr.children[c] = new TrieNode();
        }
        curr = curr.children[c];
    }
    
    curr.isWord = true;
};

/** 
 * @param {string} word
 * @return {boolean}
 */
WordDictionary.prototype.search = function(word) {
    function dfs(j, node) {
        let curr = node;
        
        for (let i = j; i < word.length; i++) {
            const c = word[i];
            
            if (c === '.') {
                for (let child of Object.values(curr.children)) {
                    if (dfs(i + 1, child)) {
                        return true;
                    }
                }
                return false;
            } else {
                if (!(c in curr.children)) {
                    return false;
                }
                curr = curr.children[c];
            }
        }
        
        return curr.isWord;
    }
    
    return dfs(0, this.root);
};
```

#### Java
```java
class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isWord = false;
}

class WordDictionary {
    private TrieNode root;

    /** Initialize your data structure here. */
    public WordDictionary() {
        root = new TrieNode();
    }
    
    public void addWord(String word) {
        TrieNode curr = root;
        
        for (char c : word.toCharArray()) {
            curr.children.putIfAbsent(c, new TrieNode());
            curr = curr.children.get(c);
        }
        
        curr.isWord = true;
    }
    
    public boolean search(String word) {
        return dfs(0, root, word);
    }
    
    private boolean dfs(int j, TrieNode node, String word) {
        TrieNode curr = node;
        
        for (int i = j; i < word.length(); i++) {
            char c = word.charAt(i);
            
            if (c == '.') {
                for (TrieNode child : curr.children.values()) {
                    if (dfs(i + 1, child, word)) {
                        return true;
                    }
                }
                return false;
            } else {
                if (!curr.children.containsKey(c)) {
                    return false;
                }
                curr = curr.children.get(c);
            }
        }
        
        return curr.isWord;
    }
}
```