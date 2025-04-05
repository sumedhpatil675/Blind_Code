# Word Search II

Given an m x n board of characters and a list of strings words, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

## Approaches

### 1. Backtracking

**Time complexity:** O(m*n*4^t+s)  
**Space complexity:** O(t)  
where m is the number of rows, n is the number of columns, t is the maximum length of any word in the array words, and s is the sum of the lengths of all the words.

#### Python
```python
class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        ROWS, COLS = len(board), len(board[0])
        res = []
        
        def backtrack(r, c, i):
            if i == len(word):
                return True
                
            if (r < 0 or c < 0 or r >= ROWS or
                c >= COLS or board[r][c] != word[i]
               ):
                return False
                
            board[r][c] = '*'
            ret = (backtrack(r + 1, c, i + 1) or
                  backtrack(r - 1, c, i + 1) or
                  backtrack(r, c + 1, i + 1) or
                  backtrack(r, c - 1, i + 1))
            board[r][c] = word[i]
            return ret
        
        for word in words:
            flag = False
            for r in range(ROWS):
                if flag:
                    break
                for c in range(COLS):
                    if board[r][c] != word[0]:
                        continue
                    if backtrack(r, c, 0):
                        res.append(word)
                        flag = True
                        break
        
        return res
```

#### JavaScript
```javascript
/**
 * @param {character[][]} board
 * @param {string[]} words
 * @return {string[]}
 */
var findWords = function(board, words) {
    const ROWS = board.length;
    const COLS = board[0].length;
    const result = [];
    
    function backtrack(r, c, i, word) {
        if (i === word.length) {
            return true;
        }
        
        if (
            r < 0 || c < 0 || 
            r >= ROWS || c >= COLS || 
            board[r][c] !== word[i]
        ) {
            return false;
        }
        
        const temp = board[r][c];
        board[r][c] = '*'; // Mark as visited
        
        const found = (
            backtrack(r + 1, c, i + 1, word) || 
            backtrack(r - 1, c, i + 1, word) || 
            backtrack(r, c + 1, i + 1, word) || 
            backtrack(r, c - 1, i + 1, word)
        );
        
        board[r][c] = temp; // Restore original character
        return found;
    }
    
    for (const word of words) {
        let found = false;
        for (let r = 0; r < ROWS && !found; r++) {
            for (let c = 0; c < COLS; c++) {
                if (board[r][c] !== word[0]) continue;
                
                if (backtrack(r, c, 0, word)) {
                    result.push(word);
                    found = true;
                    break;
                }
            }
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        int ROWS = board.length;
        int COLS = board[0].length;
        List<String> result = new ArrayList<>();
        
        for (String word : words) {
            boolean found = false;
            for (int r = 0; r < ROWS && !found; r++) {
                for (int c = 0; c < COLS; c++) {
                    if (board[r][c] != word.charAt(0)) continue;
                    
                    if (backtrack(board, word, r, c, 0)) {
                        result.add(word);
                        found = true;
                        break;
                    }
                }
            }
        }
        
        return result;
    }
    
    private boolean backtrack(char[][] board, String word, int r, int c, int i) {
        if (i == word.length()) {
            return true;
        }
        
        if (r < 0 || c < 0 || 
            r >= board.length || c >= board[0].length || 
            board[r][c] != word.charAt(i)) {
            return false;
        }
        
        char temp = board[r][c];
        board[r][c] = '*'; // Mark as visited
        
        boolean found = (
            backtrack(board, word, r + 1, c, i + 1) || 
            backtrack(board, word, r - 1, c, i + 1) || 
            backtrack(board, word, r, c + 1, i + 1) || 
            backtrack(board, word, r, c - 1, i + 1)
        );
        
        board[r][c] = temp; // Restore original character
        return found;
    }
}
```

### 2. Backtracking (Trie + Hash Set)

**Time complexity:** O(m*n*4*3^(t-1)+s)  
**Space complexity:** O(s)  
where m is the number of rows, n is the number of columns, t is the maximum length of any word in the array words, and s is the sum of the lengths of all the words.

#### Python
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isWord = False
        
    def addWord(self, word):
        cur = self
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()
            cur = cur.children[c]
        cur.isWord = True

class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        root = TrieNode()
        for w in words:
            root.addWord(w)
            
        ROWS, COLS = len(board), len(board[0])
        res, visit = set(), set()
        
        def dfs(r, c, node, word):
            if (r < 0 or c < 0 or r >= ROWS or
                c >= COLS or (r, c) in visit or
                board[r][c] not in node.children
               ):
                return
                
            visit.add((r, c))
            node = node.children[board[r][c]]
            word += board[r][c]
            
            if node.isWord:
                res.add(word)
                
            dfs(r + 1, c, node, word)
            dfs(r - 1, c, node, word)
            dfs(r, c + 1, node, word)
            dfs(r, c - 1, node, word)
            
            visit.remove((r, c))
            
        for r in range(ROWS):
            for c in range(COLS):
                dfs(r, c, root, "")
                
        return list(res)
```

The solution uses two key components:
1. A **Trie** data structure
2. **Depth-First Search (DFS)**

## Trie Data Structure

A Trie is a tree-like structure that efficiently stores a collection of strings:

```
       root
      /  |  \
     c   d   g...
    /     \
   a       o
  / \       \
 t   s       g
 |
 s (isWord=True)
```

In this example, words like "cats" and "dog" are stored in the Trie. Each node has:
- A collection of child nodes (one for each possible next character)
- A flag indicating if the path from root to this node forms a complete word

## The Algorithm Step by Step

### Step 1: Build the Trie
Insert all target words into the Trie structure.

### Step 2: Search the Board Using DFS

Let's visualize with this sample board:

```
┌───┬───┬───┐
│ C │ A │ T │
├───┼───┼───┤
│ R │ S │ E │
├───┼───┼───┤
│ D │ O │ G │
└───┴───┴───┘
```

#### DFS Illustration

Starting from cell (0,0) with letter 'C':

```
┌───┬───┬───┐
│[C]│ A │ T │
├───┼───┼───┤
│ R │ S │ E │
├───┼───┼───┤
│ D │ O │ G │
└───┴───┴───┘
```

1. We check if 'C' is in the Trie's root children: Yes
2. We mark (0,0) as visited
3. We explore adjacent cells

Moving to cell (0,1) with letter 'A':

```
┌───┬───┬───┐
│[C]│[A]│ T │
├───┼───┼───┤
│ R │ S │ E │
├───┼───┼───┤
│ D │ O │ G │
└───┴───┴───┘
```

1. We check if 'A' is in the Trie node for 'C': Yes
2. We mark (0,1) as visited
3. We explore adjacent cells

Moving to cell (0,2) with letter 'T':

```
┌───┬───┬───┐
│[C]│[A]│[T]│
├───┼───┼───┤
│ R │ S │ E │
├───┼───┼───┤
│ D │ O │ G │
└───┴───┴───┘
```

1. We check if 'T' is in the Trie node for 'CA': Yes
2. We mark (0,2) as visited
3. We check if 'CAT' is a complete word in our Trie: Yes, add to results
4. We explore adjacent cells (none valid or unvisited)
5. We backtrack, unmark (0,2)

```
┌───┬───┬───┐
│[C]│[A]│ T │
├───┼───┼───┤
│ R │ S │ E │
├───┼───┼───┤
│ D │ O │ G │
└───┴───┴───┘
```

And continue exploration from other adjacent cells of 'A'...

We repeat this process starting from every cell on the board. For example, starting from (2,0):

```
┌───┬───┬───┐
│ C │ A │ T │
├───┼───┼───┤
│ R │ S │ E │
├───┼───┼───┤
│[D]│ O │ G │
└───┴───┴───┘
```

Eventually finding "DOG":

```
┌───┬───┬───┐
│ C │ A │ T │
├───┼───┼───┤
│ R │ S │ E │
├───┼───┼───┤
│[D]│[O]│[G]│
└───┴───┴───┘
```

## The Search Process in Detail

For each cell on the board:
1. Start DFS if the cell's letter is in the Trie's root children
2. During DFS:
   - Mark cells as visited to avoid reusing them
   - Follow the path in the Trie corresponding to the sequence of letters
   - When a complete word is found, add it to results
   - Explore all four adjacent directions (up, down, left, right)
   - Backtrack by unvisiting cells when done exploring a path

## A Trace of Finding "CAT"

1. Start at cell (0,0) = 'C'
   ```
   Current Path: "C"
   Visited: [(0,0)]
   ```

2. Move to cell (0,1) = 'A'
   ```
   Current Path: "CA"
   Visited: [(0,0), (0,1)]
   ```

3. Move to cell (0,2) = 'T'
   ```
   Current Path: "CAT"
   Visited: [(0,0), (0,1), (0,2)]
   Found word: "CAT" ✓
   ```

4. Backtrack and explore other paths...
   

#### JavaScript Implementation

```javascript
/**
 * @param {string[][]} grid
 * @param {string[]} words
 * @return {string[]}
 */

class TrieNode {
  constructor() {
    this.children = {};
    this.isWord = false;
  }
}

export default function findWordsInGrid(grid, words) {
  // Create the root of our Trie
  const root = new TrieNode();

  // Function to add a word to the Trie
  function addWord(word) {
    let curr = root;
    for(let c of word) {
      if(!(c in curr.children)) {
        curr.children[c] = new TrieNode();
      }
      curr = curr.children[c];
    }
    curr.isWord = true;
  }

  // Building trie with all words
  for(let w of words) {
    addWord(w);
  }

  const ROWS = grid.length;
  const COLS = grid[0].length;
  const res = new Set();
  const visit = new Set();
  
  function dfs(r, c, node, word) {
    if(r < 0 || c < 0 || r === ROWS || c === COLS || 
       !(grid[r][c] in node.children) || 
       visit.has(`${r},${c}`)) {
      return;
    }

    visit.add(`${r},${c}`);
    node = node.children[grid[r][c]];
    word += grid[r][c];

    if(node.isWord) {
      res.add(word);
    }
    
    dfs(r+1, c, node, word);
    dfs(r-1, c, node, word);
    dfs(r, c+1, node, word);
    dfs(r, c-1, node, word);

    visit.delete(`${r},${c}`);
  }

  for(let r = 0; r < ROWS; r++) {
    for(let c = 0; c < COLS; c++) {
      dfs(r, c, root, "");
    }
  }

  return Array.from(res);
}


#### Java Implementation

```java
    class TrieNode {
        Map<Character, TrieNode> children = new HashMap<>();
        boolean isWord = false;
        
        public void addWord(String word) {
            TrieNode cur = this;
            for (char c : word.toCharArray()) {
                if (!cur.children.containsKey(c)) {
                    cur.children.put(c, new TrieNode());
                }
                cur = cur.children.get(c);
            }
            cur.isWord = true;
        }
    }
    
    public List<String> findWords(char[][] board, String[] words) {
        TrieNode root = new TrieNode();
        
        // Build the Trie with all words
        for (String w : words) {
            root.addWord(w);
        }
        
        int ROWS = board.length;
        int COLS = board[0].length;
        Set<String> res = new HashSet<>();
        Set<Pair<Integer, Integer>> visit = new HashSet<>();
        
        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                dfs(r, c, root, "", board, res, visit, ROWS, COLS);
            }
        }
        
        return new ArrayList<>(res);
    }
    
    private void dfs(int r, int c, TrieNode node, String word, char[][] board, 
                    Set<String> res, Set<Pair<Integer, Integer>> visit, 
                    int ROWS, int COLS) {
        
        if (r < 0 || c < 0 || r == ROWS || c == COLS || 
            !node.children.containsKey(board[r][c]) || 
            visit.contains(new Pair<>(r, c))) {
            return;
        }
        
        visit.add(new Pair<>(r, c));
        node = node.children.get(board[r][c]);
        word += board[r][c];
        
        if (node.isWord) {
            res.add(word);
        }
        
        dfs(r + 1, c, node, word, board, res, visit, ROWS, COLS);
        dfs(r - 1, c, node, word, board, res, visit, ROWS, COLS);
        dfs(r, c + 1, node, word, board, res, visit, ROWS, COLS);
        dfs(r, c - 1, node, word, board, res, visit, ROWS, COLS);
        
        visit.remove(new Pair<>(r, c));
    }
```


### 3. Backtracking (Trie) - More Optimized

**Time complexity:** O(m*n*4*3^(t-1)+s)  
**Space complexity:** O(s)  
where m is the number of rows, n is the number of columns, t is the maximum length of any word in the array words, and s is the sum of the lengths of all the words.

#### Python
```python
class TrieNode:
    def __init__(self):
        self.children = [None] * 26
        self.idx = -1
        self.refs = 0
        
    def addWord(self, word, i):
        cur = self
        cur.refs += 1
        for c in word:
            index = ord(c) - ord('a')
            if not cur.children[index]:
                cur.children[index] = TrieNode()
            cur = cur.children[index]
            cur.refs += 1
        cur.idx = i

class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        root = TrieNode()
        for i in range(len(words)):
            root.addWord(words[i], i)
            
        ROWS, COLS = len(board), len(board[0])
        res = []
        
        def getIndex(c):
            index = ord(c) - ord('a')
            return index
        
        def dfs(r, c, node):
            if (r < 0 or c < 0 or r >= ROWS or
                c >= COLS or board[r][c] == '*' or
                not node.children[getIndex(board[r][c])]):
                return
                
            tmp = board[r][c]
            board[r][c] = '*'
            
            prev = node
            node = node.children[getIndex(tmp)]
            
            if node.idx != -1:
                res.append(words[node.idx])
                node.idx = -1
            
            node.refs -= 1
            if not node.refs:
                prev.children[getIndex(tmp)] = None
                node = None
                board[r][c] = tmp
                return
                
            dfs(r + 1, c, node)
            dfs(r - 1, c, node)
            dfs(r, c + 1, node)
            dfs(r, c - 1, node)
            
            board[r][c] = tmp
            
        for r in range(ROWS):
            for c in range(COLS):
                dfs(r, c, root)
                
        return res
```

#### JavaScript
```javascript
class TrieNode {
    constructor() {
        this.children = Array(26).fill(null);
        this.idx = -1;
        this.refs = 0;
    }
}

/**
 * @param {character[][]} board
 * @param {string[]} words
 * @return {string[]}
 */
var findWords = function(board, words) {
    const ROWS = board.length;
    const COLS = board[0].length;
    const result = [];
    
    const root = new TrieNode();
    
    // Build Trie
    for (let i = 0; i < words.length; i++) {
        addWord(root, words[i], i);
    }
    
    function addWord(node, word, idx) {
        let curr = node;
        curr.refs++;
        
        for (const c of word) {
            const index = c.charCodeAt(0) - 'a'.charCodeAt(0);
            
            if (!curr.children[index]) {
                curr.children[index] = new TrieNode();
            }
            
            curr = curr.children[index];
            curr.refs++;
        }
        
        curr.idx = idx;
    }
    
    function getIndex(c) {
        return c.charCodeAt(0) - 'a'.charCodeAt(0);
    }
    
    function dfs(r, c, node) {
        if (
            r < 0 || c < 0 || 
            r >= ROWS || c >= COLS || 
            board[r][c] === '*' || 
            !node.children[getIndex(board[r][c])]
        ) {
            return;
        }
        
        const tmp = board[r][c];
        board[r][c] = '*'; // Mark as visited
        
        const prev = node;
        node = node.children[getIndex(tmp)];
        
        if (node.idx !== -1) {
            result.push(words[node.idx]);
            node.idx = -1;
        }
        
        node.refs--;
        if (node.refs === 0) {
            prev.children[getIndex(tmp)] = null;
            node = null;
            board[r][c] = tmp;
            return;
        }
        
        dfs(r + 1, c, node);
        dfs(r - 1, c, node);
        dfs(r, c + 1, node);
        dfs(r, c - 1, node);
        
        board[r][c] = tmp; // Restore original character
    }
    
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            dfs(r, c, root);
        }
    }
    
    return result;
};
```

#### Java
```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    int idx = -1;
    int refs = 0;
}

class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        List<String> result = new ArrayList<>();
        TrieNode root = new TrieNode();
        
        // Build Trie
        for (int i = 0; i < words.length; i++) {
            addWord(root, words[i], i);
        }
        
        int ROWS = board.length;
        int COLS = board[0].length;
        
        // Search board
        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                dfs(board, r, c, root, words, result);
            }
        }
        
        return result;
    }
    
    private void addWord(TrieNode root, String word, int idx) {
        TrieNode curr = root;
        curr.refs++;
        
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            
            if (curr.children[index] == null) {
                curr.children[index] = new TrieNode();
            }
            
            curr = curr.children[index];
            curr.refs++;
        }
        
        curr.idx = idx;
    }
    
    private void dfs(char[][] board, int r, int c, TrieNode node, String[] words, List<String> result) {
        if (r < 0 || c < 0 || 
            r >= board.length || c >= board[0].length || 
            board[r][c] == '*' || 
            node.children[board[r][c] - 'a'] == null) {
            return;
        }
        
        char tmp = board[r][c];
        board[r][c] = '*'; // Mark as visited
        
        TrieNode prev = node;
        node = node.children[tmp - 'a'];
        
        if (node.idx != -1) {
            result.add(words[node.idx]);
            node.idx = -1;
        }
        
        node.refs--;
        if (node.refs == 0) {
            prev.children[tmp - 'a'] = null;
            node = null;
            board[r][c] = tmp; // Restore original character
            return;
        }
        
        dfs(board, r + 1, c, node, words, result);
        dfs(board, r - 1, c, node, words, result);
        dfs(board, r, c + 1, node, words, result);
        dfs(board, r, c - 1, node, words, result);
        
        board[r][c] = tmp; // Restore original character
    }
}
```
