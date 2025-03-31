# Word Break

Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated sequence of one or more dictionary words.

## Approaches

### 1. Recursion
- **Time complexity**: O(2^n)
- **Space complexity**: O(n)

#### Python
```python
def wordBreak(s, wordDict):
    def dfs(i):
        if i == len(s):
            return True
        for w in wordDict:
            if ((i + len(w)) <= len(s) and
                s[i : i + len(w)] == w
            ):
                if dfs(i + len(w)):
                    return True
        return False
    return dfs(0)
```

#### JavaScript
```javascript
function wordBreak(s, wordDict) {
    function dfs(i) {
        if (i === s.length) {
            return true;
        }
        for (const word of wordDict) {
            if (i + word.length <= s.length && s.substring(i, i + word.length) === word) {
                if (dfs(i + word.length)) {
                    return true;
                }
            }
        }
        return false;
    }
    return dfs(0);
}
```

#### Java
```java
public boolean wordBreak(String s, List<String> wordDict) {
    return dfs(s, wordDict, 0);
}

private boolean dfs(String s, List<String> wordDict, int start) {
    if (start == s.length()) {
        return true;
    }
    for (String word : wordDict) {
        if (start + word.length() <= s.length() && 
            s.substring(start, start + word.length()).equals(word)) {
            if (dfs(s, wordDict, start + word.length())) {
                return true;
            }
        }
    }
    return false;
}
```

### 2. Dynamic Programming (Top-Down)
- **Time complexity**: O(n * m * t)
- **Space complexity**: O(n)

Where:
- n is the length of the string s
- m is the number of words in wordDict
- t is the maximum length of any word in wordDict

#### Python
```python
def wordBreak(s, wordDict):
    memo = {len(s): True}
    def dfs(i):
        if i in memo:
            return memo[i]
        if i == len(s):
            return True
        for w in wordDict:
            if ((i + len(w)) <= len(s) and
                s[i : i + len(w)] == w
            ):
                if dfs(i + len(w)):
                    memo[i] = True
                    return True
        memo[i] = False
        return False
    return dfs(0)
```

#### JavaScript
```javascript
function wordBreak(s, wordDict) {
    const memo = new Map();
    memo.set(s.length, true);
    
    function dfs(i) {
        if (memo.has(i)) {
            return memo.get(i);
        }
        if (i === s.length) {
            return true;
        }
        for (const word of wordDict) {
            if (i + word.length <= s.length && s.substring(i, i + word.length) === word) {
                if (dfs(i + word.length)) {
                    memo.set(i, true);
                    return true;
                }
            }
        }
        memo.set(i, false);
        return false;
    }
    return dfs(0);
}
```

#### Java
```java
public boolean wordBreak(String s, List<String> wordDict) {
    Map<Integer, Boolean> memo = new HashMap<>();
    memo.put(s.length(), true);
    return dfs(s, wordDict, 0, memo);
}

private boolean dfs(String s, List<String> wordDict, int start, Map<Integer, Boolean> memo) {
    if (memo.containsKey(start)) {
        return memo.get(start);
    }
    if (start == s.length()) {
        return true;
    }
    for (String word : wordDict) {
        if (start + word.length() <= s.length() && 
            s.substring(start, start + word.length()).equals(word)) {
            if (dfs(s, wordDict, start + word.length(), memo)) {
                memo.put(start, true);
                return true;
            }
        }
    }
    memo.put(start, false);
    return false;
}
```

### 3. Dynamic Programming (Bottom-Up)
- **Time complexity**: O(n * m * t)
- **Space complexity**: O(n)

#### Python
```python
def wordBreak(s, wordDict):
    dp = [False] * (len(s) + 1)
    dp[len(s)] = True
    for i in range(len(s) - 1, -1, -1):
        for w in wordDict:
            if (i + len(w)) <= len(s) and s[i : i + len(w)] == w:
                dp[i] = dp[i + len(w)]
            if dp[i]:
                break
    return dp[0]
```

#### JavaScript
```javascript
function wordBreak(s, wordDict) {
    const dp = new Array(s.length + 1).fill(false);
    dp[s.length] = true;
    
    for (let i = s.length - 1; i >= 0; i--) {
        for (const word of wordDict) {
            if (i + word.length <= s.length && s.substring(i, i + word.length) === word) {
                dp[i] = dp[i + word.length];
                if (dp[i]) break;
            }
        }
    }
    return dp[0];
}
```

#### Java
```java
public boolean wordBreak(String s, List<String> wordDict) {
    boolean[] dp = new boolean[s.length() + 1];
    dp[s.length()] = true;
    
    for (int i = s.length() - 1; i >= 0; i--) {
        for (String word : wordDict) {
            if (i + word.length() <= s.length() && 
                s.substring(i, i + word.length()).equals(word)) {
                dp[i] = dp[i + word.length()];
                if (dp[i]) break;
            }
        }
    }
    return dp[0];
}
```

### 4. Dynamic Programming with Trie
- **Time complexity**: O((n * t^2) + m)
- **Space complexity**: O(n + (m * t))

Where:
- n is the length of the string s
- m is the number of words in wordDict
- t is the maximum length of any word in wordDict

#### Python
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
        
    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_word = True
    
    def search(self, s, i, j):
        node = self.root
        for idx in range(i, j + 1):
            if s[idx] not in node.children:
                return False
            node = node.children[s[idx]]
        return node.is_word

def wordBreak(s, wordDict):
    trie = Trie()
    for word in wordDict:
        trie.insert(word)
        
    dp = [False] * (len(s) + 1)
    dp[len(s)] = True
    
    t = 0
    for w in wordDict:
        t = max(t, len(w))
        
    for i in range(len(s), -1, -1):
        for j in range(i, min(len(s), i + t)):
            if trie.search(s, i, j):
                dp[i] = dp[j + 1]
                if dp[i]:
                    break
                    
    return dp[0]
```

#### JavaScript
```javascript
class TrieNode {
    constructor() {
        this.children = {};
        this.isWord = false;
    }
}

class Trie {
    constructor() {
        this.root = new TrieNode();
    }
    
    insert(word) {
        let node = this.root;
        for (const char of word) {
            if (!node.children[char]) {
                node.children[char] = new TrieNode();
            }
            node = node.children[char];
        }
        node.isWord = true;
    }
    
    search(s, i, j) {
        let node = this.root;
        for (let idx = i; idx <= j; idx++) {
            if (!node.children[s[idx]]) {
                return false;
            }
            node = node.children[s[idx]];
        }
        return node.isWord;
    }
}

function wordBreak(s, wordDict) {
    const trie = new Trie();
    for (const word of wordDict) {
        trie.insert(word);
    }
    
    const dp = new Array(s.length + 1).fill(false);
    dp[s.length] = true;
    
    let maxWordLength = 0;
    for (const word of wordDict) {
        maxWordLength = Math.max(maxWordLength, word.length);
    }
    
    for (let i = s.length - 1; i >= 0; i--) {
        for (let j = i; j < Math.min(s.length, i + maxWordLength); j++) {
            if (trie.search(s, i, j) && dp[j + 1]) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[0];
}
```

#### Java
```java
class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isWord = false;
}

class Trie {
    TrieNode root = new TrieNode();
    
    public void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (!node.children.containsKey(c)) {
                node.children.put(c, new TrieNode());
            }
            node = node.children.get(c);
        }
        node.isWord = true;
    }
    
    public boolean search(String s, int start, int end) {
        TrieNode node = root;
        for (int i = start; i <= end; i++) {
            char c = s.charAt(i);
            if (!node.children.containsKey(c)) {
                return false;
            }
            node = node.children.get(c);
        }
        return node.isWord;
    }
}

public boolean wordBreak(String s, List<String> wordDict) {
    Trie trie = new Trie();
    int maxWordLength = 0;
    
    for (String word : wordDict) {
        trie.insert(word);
        maxWordLength = Math.max(maxWordLength, word.length());
    }
    
    boolean[] dp = new boolean[s.length() + 1];
    dp[s.length()] = true;
    
    for (int i = s.length() - 1; i >= 0; i--) {
        for (int j = i; j < Math.min(s.length(), i + maxWordLength); j++) {
            if (trie.search(s, i, j) && dp[j + 1]) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[0];
}
```