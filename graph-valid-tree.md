# Graph Valid Tree

Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

## Approaches

### 1. Cycle Detection (DFS) ✅

**Time complexity:** O(V+E)  
**Space complexity:** O(V+E)  
where V is the number of vertices and E is the number of edges in the graph.

#### Python
```python
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        if len(edges) > (n - 1):
            return False
            
        adj = [[] for _ in range(n)]
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)
            
        visit = set()
        
        def dfs(node, par):
            if node in visit:
                return False
                
            visit.add(node)
            
            for nei in adj[node]:
                if nei == par:
                    continue
                    
                if not dfs(nei, node):
                    return False
                    
            return True
            
        return dfs(0, -1) and len(visit) == n
```

## ASCII Diagram Explanation

This algorithm determines if a given undirected graph is a valid tree. For a graph to be a valid tree:
1. It must be fully connected (all nodes reachable)
2. It must have no cycles

```
Valid Tree:               Not a Tree (Has Cycle):      Not a Tree (Disconnected):

    0                          0                             0        3
   / \                        / \                           / \       |
  1   2                      1---2                         1   2      4
 /                           |
3                            3
```

## Tracking Table Example

For the valid tree example above:

```
+--------+---------------+----------------------+---------------+
| Step   | Current Node  | Previous Node        | Visited Set   |
+--------+---------------+----------------------+---------------+
| Start  | 0             | -1                   | {}            |
| 1      | 0             | -1                   | {0}           |
| 2      | 1             | 0                    | {0,1}         |
| 3      | 3             | 1                    | {0,1,3}       |
| 4      | 2             | 0                    | {0,1,3,2}     |
| Final  | Check: n=4 == len(visit)=4           | True          |
+--------+---------------+----------------------+---------------+
```

For the cycle example:

```
+--------+---------------+----------------------+---------------+
| Step   | Current Node  | Previous Node        | Visited Set   |
+--------+---------------+----------------------+---------------+
| Start  | 0             | -1                   | {}            |
| 1      | 0             | -1                   | {0}           |
| 2      | 1             | 0                    | {0,1}         |
| 3      | 2             | 1                    | {0,1,2}       |
| 4      | 0             | 2                    | {0,1,2}       |
| 5      | Already visited 0 → Cycle detected!  | Return False  |
+--------+---------------+----------------------+---------------+
```

## Example Execution Trace

Let's trace the algorithm on a simple 4-node tree:
```
    0
   / \
  1   2
 /
3
```

With edges list: [[0,1], [0,2], [1,3]]

```
1. Initialize:
   - n = 4
   - adj = {0:[], 1:[], 2:[], 3:[]}

2. Build adjacency list:
   - adj = {0:[1,2], 1:[0,3], 2:[0], 3:[1]}
   - visit = {}

3. Start DFS from node 0:
   - dfs(0, -1)
     - Mark 0 as visited: visit = {0}
     - For neighbor 1:
       - dfs(1, 0)
         - Mark 1 as visited: visit = {0,1}
         - For neighbor 0:
           - 0 == prev, so skip
         - For neighbor 3:
           - dfs(3, 1)
             - Mark 3 as visited: visit = {0,1,3}
             - For neighbor 1:
               - 1 == prev, so skip
             - Return True
         - Return True
     - For neighbor 2:
       - dfs(2, 0)
         - Mark 2 as visited: visit = {0,1,3,2}
         - For neighbor 0:
           - 0 == prev, so skip
         - Return True
     - Return True

4. Final check:
   - dfs(0, -1) returned True
   - n = 4 and len(visit) = 4, so 4 == 4 is True
   - Return True (valid tree confirmed)
```

## Key Insights

This algorithm leverages two critical properties of trees:
1. **No cycles**: The DFS checks for cycles by keeping track of visited nodes and ensuring no previously visited node (except the direct parent) is encountered again.
2. **Fully connected**: After the DFS, all nodes must be visited (n == len(visit)) to confirm there are no disconnected components.
   

#### JavaScript
```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {boolean}
 */
var validTree = function(n, edges) {
    // A valid tree has exactly n-1 edges
    if (edges.length !== n - 1) {
        return false;
    }
    
    // Create adjacency list
    const adj = Array(n).fill().map(() => []);
    for (const [u, v] of edges) {
        adj[u].push(v);
        adj[v].push(u);
    }
    
    const visited = new Set();
    
    function dfs(node, parent) {
        if (visited.has(node)) {
            return false;
        }
        
        visited.add(node);
        
        for (const neighbor of adj[node]) {
            if (neighbor === parent) {
                continue;
            }
            
            if (!dfs(neighbor, node)) {
                return false;
            }
        }
        
        return true;
    }
    
    return dfs(0, -1) && visited.size === n;
};
```

#### Java
```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        // A valid tree has exactly n-1 edges
        if (edges.length != n - 1) {
            return false;
        }
        
        // Create adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
        
        Set<Integer> visited = new HashSet<>();
        
        return dfs(0, -1, adj, visited) && visited.size() == n;
    }
    
    private boolean dfs(int node, int parent, List<List<Integer>> adj, Set<Integer> visited) {
        if (visited.contains(node)) {
            return false;
        }
        
        visited.add(node);
        
        for (int neighbor : adj.get(node)) {
            if (neighbor == parent) {
                continue;
            }
            
            if (!dfs(neighbor, node, adj, visited)) {
                return false;
            }
        }
        
        return true;
    }
}
```

### 2. Breadth First Search

**Time complexity:** O(V+E)  
**Space complexity:** O(V+E)  
where V is the number of vertices and E is the number of edges in the graph.

#### Python
```python
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        if len(edges) > n - 1:
            return False
            
        adj = [[] for _ in range(n)]
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)
            
        visit = set()
        q = deque([(0, -1)]) # (current node, parent node)
        visit.add(0)
        
        while q:
            node, parent = q.popleft()
            
            for nei in adj[node]:
                if nei == parent:
                    continue
                    
                if nei in visit:
                    return False
                    
                visit.add(nei)
                q.append((nei, node))
                
        return len(visit) == n
```

#### JavaScript
```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {boolean}
 */
var validTree = function(n, edges) {
    if (edges.length !== n - 1) {
        return false;
    }
    
    // Create adjacency list
    const adj = Array(n).fill().map(() => []);
    for (const [u, v] of edges) {
        adj[u].push(v);
        adj[v].push(u);
    }
    
    const visited = new Set([0]);
    const queue = [[0, -1]]; // [current node, parent node]
    
    while (queue.length > 0) {
        const [node, parent] = queue.shift();
        
        for (const neighbor of adj[node]) {
            if (neighbor === parent) {
                continue;
            }
            
            if (visited.has(neighbor)) {
                return false;
            }
            
            visited.add(neighbor);
            queue.push([neighbor, node]);
        }
    }
    
    return visited.size === n;
};
```

#### Java
```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        if (edges.length != n - 1) {
            return false;
        }
        
        // Create adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
        
        Set<Integer> visited = new HashSet<>();
        Queue<int[]> queue = new LinkedList<>(); // [node, parent]
        
        visited.add(0);
        queue.offer(new int[]{0, -1});
        
        while (!queue.isEmpty()) {
            int[] pair = queue.poll();
            int node = pair[0];
            int parent = pair[1];
            
            for (int neighbor : adj.get(node)) {
                if (neighbor == parent) {
                    continue;
                }
                
                if (visited.contains(neighbor)) {
                    return false;
                }
                
                visited.add(neighbor);
                queue.offer(new int[]{neighbor, node});
            }
        }
        
        return visited.size() == n;
    }
}
```

### 3. Disjoint Set Union (Rank | Size)

**Time complexity:** O(V+(E*α(V)))  
**Space complexity:** O(V)  
where V is the number of vertices and E is the number of edges in the graph. α() is used for amortized complexity.

#### Python
```python
class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [1] * n
        
    def find(self, node):
        cur = node
        while cur != self.parent[cur]:
            self.parent[cur] = self.parent[self.parent[cur]]
            cur = self.parent[cur]
        return cur
        
    def union(self, u, v):
        pu = self.find(u)
        pv = self.find(v)
        
        if pu == pv:
            return False
            
        if self.rank[pv] > self.rank[pu]:
            pu, pv = pv, pu
            
        self.parent[pv] = pu
        self.rank[pu] += self.rank[pv]
        return True

class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        dsu = DSU(n)
        
        for u, v in edges:
            if not dsu.union(u, v):
                return False
                
        # Check if all nodes are connected
        root = dsu.find(0)
        for i in range(1, n):
            if dsu.find(i) != root:
                return False
                
        return True
```

#### JavaScript
```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {boolean}
 */
var validTree = function(n, edges) {
    // A valid tree has exactly n-1 edges
    if (edges.length !== n - 1) {
        return false;
    }
    
    class DSU {
        constructor(n) {
            this.parent = Array(n).fill().map((_, i) => i);
            this.rank = Array(n).fill(1);
        }
        
        find(node) {
            if (this.parent[node] !== node) {
                this.parent[node] = this.find(this.parent[node]);
            }
            return this.parent[node];
        }
        
        union(u, v) {
            const pu = this.find(u);
            const pv = this.find(v);
            
            if (pu === pv) {
                return false;
            }
            
            if (this.rank[pu] < this.rank[pv]) {
                this.parent[pu] = pv;
                this.rank[pv] += this.rank[pu];
            } else {
                this.parent[pv] = pu;
                this.rank[pu] += this.rank[pv];
            }
            
            return true;
        }
    }
    
    const dsu = new DSU(n);
    
    for (const [u, v] of edges) {
        if (!dsu.union(u, v)) {
            return false;
        }
    }
    
    return true;
};
```

#### Java
```java
class Solution {
    class DSU {
        int[] parent;
        int[] rank;
        
        public DSU(int n) {
            parent = new int[n];
            rank = new int[n];
            
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                rank[i] = 1;
            }
        }
        
        public int find(int node) {
            if (parent[node] != node) {
                parent[node] = find(parent[node]);
            }
            return parent[node];
        }
        
        public boolean union(int u, int v) {
            int pu = find(u);
            int pv = find(v);
            
            if (pu == pv) {
                return false;
            }
            
            if (rank[pu] < rank[pv]) {
                parent[pu] = pv;
                rank[pv] += rank[pu];
            } else {
                parent[pv] = pu;
                rank[pu] += rank[pv];
            }
            
            return true;
        }
    }
    
    public boolean validTree(int n, int[][] edges) {
        // A valid tree has exactly n-1 edges
        if (edges.length != n - 1) {
            return false;
        }
        
        DSU dsu = new DSU(n);
        
        for (int[] edge : edges) {
            if (!dsu.union(edge[0], edge[1])) {
                return false;
            }
        }
        
        return true;
    }
}
```
