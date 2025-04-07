# Word Break

Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated sequence of one or more dictionary words.

## Approaches

### 1. Recursion
- **Time complexity**: O(2^n)
  - n is the length of the string s
- **Space complexity**: O(n)
  - n is the length of the string s (recursion stack depth)

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
  - n is the length of the string s
  - m is the number of words in wordDict
  - t is the maximum length of any word in wordDict
- **Space complexity**: O(n)
  - n is the length of the string s (memoization array size)

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

### 3. Dynamic Programming (Bottom-Up) ✅✅✅

- **Time complexity**: O(n * m * t)
  - n is the length of the string s
  - m is the number of words in wordDict
  - t is the maximum length of any word in wordDict
- **Space complexity**: O(n)
  - n is the length of the string s (DP array size)

#### Python
```python
def wordBreak(s, wordDict):
    dp = [False] * (len(s) + 1)
    dp[len(s)] = True
    for i in range(len(s) - 1, -1, -1):
        for w in wordDict:
            extractedWord = s[i:i+len(w)]
            if (i + len(w)) <= len(s) and extractedWord == w:
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
            const extractedWord = s.substring(i, i + word.length);
            if (i + word.length <= s.length && extractedWord === word) {
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
            String extractedWord = i + word.length() <= s.length() ? 
                                  s.substring(i, i + word.length()) : "";
            if (i + word.length() <= s.length() && extractedWord.equals(word)) {
                dp[i] = dp[i + word.length()];
                if (dp[i]) break;
            }
        }
    }
    return dp[0];
}
```

## DP Table Visualization

Let's visualize how the DP table gets populated for the input string `"leetcode"` with wordDict = ["leet", "code"].

```
String: l e e t c o d e
Index:  0 1 2 3 4 5 6 7 8
        
DP[8] = true (base case for empty string)
```

Starting from index 7 (last character 'e'):
```
Index:  0 1 2 3 4 5 6 7 8
String: l e e t c o d e
DP:     ? ? ? ? ? ? ? ? T
```

Checking index 7 (character 'e'):
- "e" is not in wordDict
- After checking all words, dp[7] remains false

```
Index:  0 1 2 3 4 5 6 7 8
String: l e e t c o d e
DP:     ? ? ? ? ? ? ? F T
```

Checking index 6 (character 'd'):
- "d" is not in wordDict
- After checking all words, dp[6] remains false

```
Index:  0 1 2 3 4 5 6 7 8
String: l e e t c o d e
DP:     ? ? ? ? ? ? F F T
```

Continuing for each index...

Checking index 4 (character 'c'):
- Check "code" (from index 4 to 7)
- extractedWord = "code", which matches a word in wordDict
- dp[4] = dp[4 + 4] = dp[8] = true

```
Index:  0 1 2 3 4 5 6 7 8
String: l e e t c o d e
DP:     ? ? ? ? T F F F T
```

Checking index 3 (character 't'):
- No matching word starts at this position

```
Index:  0 1 2 3 4 5 6 7 8
String: l e e t c o d e
DP:     ? ? ? F T F F F T
```

Checking index 0 (character 'l'):
- Check "leet" (from index 0 to 3)
- extractedWord = "leet", which matches a word in wordDict
- dp[0] = dp[0 + 4] = dp[4] = true

Final DP table:
```
Index:  0 1 2 3 4 5 6 7 8
String: l e e t c o d e
DP:     T F F F T F F F T
```

Since dp[0] is true, the string "leetcode" can be segmented into words from the dictionary ["leet", "code"].

### 4. Dynamic Programming with Trie
- **Time complexity**: O((n * t^2) + m)
  - n is the length of the string s
  - m is the number of words in wordDict
  - t is the maximum length of any word in wordDict
- **Space complexity**: O(n + (m * t))
  - n is the length of the string s (DP array size)
  - m is the number of words in wordDict
  - t is the maximum length of any word in wordDict (Trie structure size)

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
