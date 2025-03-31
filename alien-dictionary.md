# Alien Dictionary

There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of non-empty words from the dictionary, where words are sorted lexicographically by the rules of this new language. Derive the order of letters in this language.

You may assume all letters are in lowercase.

The dictionary is invalid, if string a is prefix of string b and b is appear before a.

If the order is invalid, return an empty string.

There may be multiple valid order of letters, return the smallest in normal lexicographical order.

The letters in one string are of the same rank by default and are sorted in Human dictionary order.

## Approaches

### 1. Depth First Search

**Time complexity:** O(N+V+E)  
**Space complexity:** O(V+E)  
where V is the number of unique characters, E is the number of edges and N is the sum of lengths of all the strings.

#### Python
```python
class Solution:
    def foreignDictionary(self, words: List[str]) -> str:
        adj = {c: set() for w in words for c in w}
        
        for i in range(len(words) - 1):
            w1, w2 = words[i], words[i + 1]
            minLen = min(len(w1), len(w2))
            
            if len(w1) > len(w2) and w1[:minLen] == w2[:minLen]:
                return ""
                
            for j in range(minLen):
                if w1[j] != w2[j]:
                    adj[w1[j]].add(w2[j])
                    break
                    
        visited = {}
        res = []
        
        def dfs(char):
            if char in visited:
                return visited[char]
                
            visited[char] = True
            
            for neighChar in adj[char]:
                if dfs(neighChar):
                    return True
                    
            visited[char] = False
            res.append(char)
            
        for char in adj:
            if dfs(char):
                return ""
                
        res.reverse()
        return "".join(res)
```

#### JavaScript
```javascript
/**
 * @param {string[]} words
 * @return {string}
 */
var alienOrder = function(words) {
    // Build adjacency list with all characters
    const adj = {};
    for (const word of words) {
        for (const c of word) {
            if (!(c in adj)) {
                adj[c] = new Set();
            }
        }
    }
    
    // Add edges to the adjacency list
    for (let i = 0; i < words.length - 1; i++) {
        const word1 = words[i];
        const word2 = words[i + 1];
        const minLen = Math.min(word1.length, word2.length);
        
        // Check for invalid case: word1 is prefix of word2 but appears before it
        if (word1.length > word2.length && word1.substring(0, minLen) === word2) {
            return "";
        }
        
        // Find the first differing character and add edge
        for (let j = 0; j < minLen; j++) {
            if (word1[j] !== word2[j]) {
                adj[word1[j]].add(word2[j]);
                break;
            }
        }
    }
    
    // DFS to detect cycles and build result
    const visited = {};  // false = visited, true = in current path
    const result = [];
    
    function dfs(char) {
        if (char in visited) {
            return visited[char]; // Return true if there's a cycle
        }
        
        visited[char] = true; // Mark as in current path
        
        for (const neighbor of adj[char]) {
            if (dfs(neighbor)) {
                return true; // Propagate cycle detection
            }
        }
        
        visited[char] = false; // Mark as visited but not in current path
        result.push(char);
        return false;
    }
    
    for (const char in adj) {
        if (dfs(char)) {
            return ""; // Cycle found
        }
    }
    
    return result.reverse().join("");
};
```

#### Java
```java
class Solution {
    public String alienOrder(String[] words) {
        // Build adjacency list with all characters
        Map<Character, Set<Character>> adj = new HashMap<>();
        for (String word : words) {
            for (char c : word.toCharArray()) {
                adj.putIfAbsent(c, new HashSet<>());
            }
        }
        
        // Add edges to the adjacency list
        for (int i = 0; i < words.length - 1; i++) {
            String word1 = words[i];
            String word2 = words[i + 1];
            int minLen = Math.min(word1.length(), word2.length());
            
            // Check for invalid case
            if (word1.length() > word2.length() && word1.substring(0, minLen).equals(word2)) {
                return "";
            }
            
            // Find the first differing character and add edge
            for (int j = 0; j < minLen; j++) {
                if (word1.charAt(j) != word2.charAt(j)) {
                    adj.get(word1.charAt(j)).add(word2.charAt(j));
                    break;
                }
            }
        }
        
        // DFS to detect cycles and build result
        Map<Character, Boolean> visited = new HashMap<>();
        List<Character> result = new ArrayList<>();
        
        for (char c : adj.keySet()) {
            if (detectCycle(c, adj, visited, result)) {
                return "";
            }
        }
        
        Collections.reverse(result);
        StringBuilder sb = new StringBuilder();
        for (char c : result) {
            sb.append(c);
        }
        
        return sb.toString();
    }
    
    private boolean detectCycle(char node, Map<Character, Set<Character>> adj, 
                               Map<Character, Boolean> visited, List<Character> result) {
        if (visited.containsKey(node)) {
            return visited.get(node); // Return true if there's a cycle
        }
        
        visited.put(node, true); // Mark as in current path
        
        for (char neighbor : adj.get(node)) {
            if (detectCycle(neighbor, adj, visited, result)) {
                return true;
            }
        }
        
        visited.put(node, false); // Mark as visited but not in current path
        result.add(node);
        return false;
    }
}
```

### 2. Topological Sort (Kahn's Algorithm)

**Time complexity:** O(N+V+E)  
**Space complexity:** O(V+E)  
where V is the number of unique characters, E is the number of edges and N is the sum of lengths of all the strings.

#### Python
```python
class Solution:
    def foreignDictionary(self, words):
        adj = {c: set() for w in words for c in w}
        indegree = {c: 0 for c in adj}
        
        for i in range(len(words) - 1):
            w1, w2 = words[i], words[i + 1]
            minLen = min(len(w1), len(w2))
            
            if len(w1) > len(w2) and w1[:minLen] == w2[:minLen]:
                return ""
                
            for j in range(minLen):
                if w1[j] != w2[j]:
                    if w2[j] not in adj[w1[j]]:
                        adj[w1[j]].add(w2[j])
                        indegree[w2[j]] += 1
                    break
                    
        q = deque([c for c in indegree if indegree[c] == 0])
        res = []
        
        while q:
            char = q.popleft()
            res.append(char)
            
            for neighbor in adj[char]:
                indegree[neighbor] -= 1
                if indegree[neighbor] == 0:
                    q.append(neighbor)
                    
        if len(res) != len(indegree):
            return ""
            
        return "".join(res)
```

#### JavaScript
```javascript
/**
 * @param {string[]} words
 * @return {string}
 */
var alienOrder = function(words) {
    // Build adjacency list and indegree map
    const adj = {};
    const indegree = {};
    
    // Initialize structures with all characters
    for (const word of words) {
        for (const c of word) {
            if (!(c in adj)) {
                adj[c] = new Set();
                indegree[c] = 0;
            }
        }
    }
    
    // Add edges and update indegrees
    for (let i = 0; i < words.length - 1; i++) {
        const word1 = words[i];
        const word2 = words[i + 1];
        const minLen = Math.min(word1.length, word2.length);
        
        // Check for invalid case
        if (word1.length > word2.length && word1.substring(0, minLen) === word2) {
            return "";
        }
        
        // Find the first differing character
        for (let j = 0; j < minLen; j++) {
            if (word1[j] !== word2[j]) {
                if (!adj[word1[j]].has(word2[j])) {
                    adj[word1[j]].add(word2[j]);
                    indegree[word2[j]]++;
                }
                break;
            }
        }
    }
    
    // Find all sources (nodes with indegree 0)
    const queue = [];
    for (const c in indegree) {
        if (indegree[c] === 0) {
            queue.push(c);
        }
    }
    
    // Perform topological sort
    const result = [];
    
    while (queue.length > 0) {
        // Sort queue to get lexicographically smallest ordering
        queue.sort();
        const char = queue.shift();
        result.push(char);
        
        for (const neighbor of adj[char]) {
            indegree[neighbor]--;
            if (indegree[neighbor] === 0) {
                queue.push(neighbor);
            }
        }
    }
    
    // Check if we visited all nodes (no cycles)
    if (result.length !== Object.keys(indegree).length) {
        return "";
    }
    
    return result.join("");
};
```

#### Java
```java
class Solution {
    public String alienOrder(String[] words) {
        // Build adjacency list and indegree map
        Map<Character, Set<Character>> adj = new HashMap<>();
        Map<Character, Integer> indegree = new HashMap<>();
        
        // Initialize data structures with all characters
        for (String word : words) {
            for (char c : word.toCharArray()) {
                adj.putIfAbsent(c, new HashSet<>());
                indegree.putIfAbsent(c, 0);
            }
        }
        
        // Add edges and update indegrees
        for (int i = 0; i < words.length - 1; i++) {
            String word1 = words[i];
            String word2 = words[i + 1];
            int minLen = Math.min(word1.length(), word2.length());
            
            // Check for invalid case
            if (word1.length() > word2.length() && word1.substring(0, minLen).equals(word2)) {
                return "";
            }
            
            // Find the first differing character
            for (int j = 0; j < minLen; j++) {
                char c1 = word1.charAt(j);
                char c2 = word2.charAt(j);
                
                if (c1 != c2) {
                    if (!adj.get(c1).contains(c2)) {
                        adj.get(c1).add(c2);
                        indegree.put(c2, indegree.get(c2) + 1);
                    }
                    break;
                }
            }
        }
        
        // Perform topological sort using priority queue for lexicographical ordering
        PriorityQueue<Character> pq = new PriorityQueue<>();
        for (char c : indegree.keySet()) {
            if (indegree.get(c) == 0) {
                pq.offer(c);
            }
        }
        
        StringBuilder result = new StringBuilder();
        
        while (!pq.isEmpty()) {
            char curr = pq.poll();
            result.append(curr);
            
            for (char neighbor : adj.get(curr)) {
                indegree.put(neighbor, indegree.get(neighbor) - 1);
                if (indegree.get(neighbor) == 0) {
                    pq.offer(neighbor);
                }
            }
        }
        
        // Check if we visited all nodes (no cycles)
        if (result.length() != indegree.size()) {
            return "";
        }
        
        return result.toString();
    }
}
```