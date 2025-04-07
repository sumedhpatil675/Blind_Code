# Number of Connected Components in an Undirected Graph

There is an undirected graph with n nodes. There is also an edges array, where edges[i] = [a, b] means that there is an edge between node a and node b in the graph.

The nodes are numbered from 0 to n - 1.

Return the total number of connected components in that graph.

## Approaches

### 1. Depth First Search ✅✅✅

**Time complexity:** O(V+E)  
**Space complexity:** O(V+E)  
where V is the number of vertices and E is the number of edges in the graph.

#### Python
```python
class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        adj = [[] for _ in range(n)]
        visit = [False] * n
        
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)
            
        def dfs(node):
            for nei in adj[node]:
                if not visit[nei]:
                    visit[nei] = True
                    dfs(nei)
                    
        res = 0
        for node in range(n):
            if not visit[node]:
                visit[node] = True
                dfs(node)
                res += 1
                
        return res
```

#### JavaScript
```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {number}
 */
var countComponents = function(n, edges) {
    // Create adjacency list
    const adj = Array(n).fill().map(() => []);
    const visited = Array(n).fill(false);
    
    for (const [u, v] of edges) {
        adj[u].push(v);
        adj[v].push(u);
    }
    
    function dfs(node) {
        for (const neighbor of adj[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                dfs(neighbor);
            }
        }
    }
    
    let components = 0;
    
    for (let i = 0; i < n; i++) {
        if (!visited[i]) {
            visited[i] = true;
            dfs(i);
            components++;
        }
    }
    
    return components;
};
```

#### Java
```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        List<List<Integer>> adj = new ArrayList<>();
        boolean[] visited = new boolean[n];
        
        // Initialize adjacency list
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        
        // Build adjacency list
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
        
        int components = 0;
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i, adj, visited);
                components++;
            }
        }
        
        return components;
    }
    
    private void dfs(int node, List<List<Integer>> adj, boolean[] visited) {
        visited[node] = true;
        
        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                dfs(neighbor, adj, visited);
            }
        }
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
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        adj = [[] for _ in range(n)]
        visit = [False] * n
        
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)
            
        def bfs(node):
            q = deque([node])
            visit[node] = True
            
            while q:
                cur = q.popleft()
                
                for nei in adj[cur]:
                    if not visit[nei]:
                        visit[nei] = True
                        q.append(nei)
                        
        res = 0
        for node in range(n):
            if not visit[node]:
                bfs(node)
                res += 1
                
        return res
```

#### JavaScript
```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {number}
 */
var countComponents = function(n, edges) {
    // Create adjacency list
    const adj = Array(n).fill().map(() => []);
    const visited = Array(n).fill(false);
    
    for (const [u, v] of edges) {
        adj[u].push(v);
        adj[v].push(u);
    }
    
    function bfs(node) {
        const queue = [node];
        visited[node] = true;
        
        while (queue.length > 0) {
            const current = queue.shift();
            
            for (const neighbor of adj[current]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.push(neighbor);
                }
            }
        }
    }
    
    let components = 0;
    
    for (let i = 0; i < n; i++) {
        if (!visited[i]) {
            bfs(i);
            components++;
        }
    }
    
    return components;
};
```

#### Java
```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        List<List<Integer>> adj = new ArrayList<>();
        boolean[] visited = new boolean[n];
        
        // Initialize adjacency list
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        
        // Build adjacency list
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
        
        int components = 0;
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                bfs(i, adj, visited);
                components++;
            }
        }
        
        return components;
    }
    
    private void bfs(int start, List<List<Integer>> adj, boolean[] visited) {
        Queue<Integer> queue = new LinkedList<>();
        visited[start] = true;
        queue.offer(start);
        
        while (!queue.isEmpty()) {
            int node = queue.poll();
            
            for (int neighbor : adj.get(node)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(neighbor);
                }
            }
        }
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
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        dsu = DSU(n)
        res = n
        
        for u, v in edges:
            if dsu.union(u, v):
                res -= 1
                
        return res
```

#### JavaScript
```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {number}
 */
var countComponents = function(n, edges) {
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
    let components = n;
    
    for (const [u, v] of edges) {
        if (dsu.union(u, v)) {
            components--;
        }
    }
    
    return components;
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
    
    public int countComponents(int n, int[][] edges) {
        DSU dsu = new DSU(n);
        int components = n;
        
        for (int[] edge : edges) {
            if (dsu.union(edge[0], edge[1])) {
                components--;
            }
        }
        
        return components;
    }
}
```
