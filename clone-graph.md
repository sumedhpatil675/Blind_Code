# Clone Graph

Given a reference of a node in a connected undirected graph, return a deep copy (clone) of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

## Approaches

### 1. Depth First Search ✅

**Time complexity:** O(V+E)  
**Space complexity:** O(V)  
where V is the number of vertices and E is the number of edges.

#### Python
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        oldToNew = {}
        
        def dfs(node):
            if node in oldToNew:
                return oldToNew[node]
                
            copy = Node(node.val)
            oldToNew[node] = copy
            
            for nei in node.neighbors:
                copy.neighbors.append(dfs(nei))
                
            return copy
            
        return dfs(node) if node else None
```

## ASCII Diagram Explanation

The function `cloneGraph` creates a deep copy of a graph while preserving its structure.

```
Original Graph:          Cloning Process:                   New Graph:
                         
    1 --- 2              1. Visit node 1                        1' --- 2'
    |     |              2. Create copy 1'                      |      |
    |     |              3. Visit neighbors                     |      |
    4 --- 3              4. Recursively clone                   4' --- 3'
```

## Tracking Table

Here's the key tracking table during execution:

```
+--------+---------------+------------------------+
| Step   | oldToNew Map  | Current Action         |
+--------+---------------+------------------------+
| Start  | {}            | Call dfs(node 1)       |
| 1      | {1: 1'}       | Create copy of node 1  |
| 2      | {1: 1'}       | Process neighbors of 1 |
| 3      | {1: 1', 2: 2'}| Clone neighbor 2       |
| 4      | {..., 3: 3'}  | Clone neighbor 3       |
| 5      | {..., 4: 4'}  | Clone neighbor 4       |
| Final  | {1:1',2:2',   | Return new copy 1'     |
|        |  3:3',4:4'}   |                        |
+--------+---------------+------------------------+
```

## Example Execution

```
Example execution for graph with nodes 1-2-3:

1. dfs(1)
   - Create copy 1'
   - oldToNew = {1: 1'}
   - For neighbor 2:
     
2.   dfs(2)
     - Create copy 2'
     - oldToNew = {1: 1', 2: 2'}
     - For neighbor 1:
       - Already in map, return 1'
     - For neighbor 3:
       
3.     dfs(3)
       - Create copy 3'
       - oldToNew = {1: 1', 2: 2', 3: 3'}
       - For neighbor 2:
         - Already in map, return 2'
       - Return 3'
       
     - Return 2'
     
   - Return 1'
```


#### JavaScript
```javascript
/**
 * // Definition for a Node.
 * function Node(val, neighbors) {
 *    this.val = val === undefined ? 0 : val;
 *    this.neighbors = neighbors === undefined ? [] : neighbors;
 * };
 */

/**
 * @param {Node} node
 * @return {Node}
 */
var cloneGraph = function(node) {
    const oldToNew = new Map();
    
    function dfs(node) {
        if (oldToNew.has(node)) {
            return oldToNew.get(node);
        }
        
        const copy = new Node(node.val);
        oldToNew.set(node, copy);
        
        for (const nei of node.neighbors) {
            copy.neighbors.push(dfs(nei));
        }
        
        return copy;
    }
    
    return node ? dfs(node) : null;
};
```

#### Java
```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return null;
        }
        
        Map<Node, Node> oldToNew = new HashMap<>();
        return dfs(node, oldToNew);
    }
    
    private Node dfs(Node node, Map<Node, Node> oldToNew) {
        if (oldToNew.containsKey(node)) {
            return oldToNew.get(node);
        }
        
        Node copy = new Node(node.val);
        oldToNew.put(node, copy);
        
        for (Node nei : node.neighbors) {
            copy.neighbors.add(dfs(nei, oldToNew));
        }
        
        return copy;
    }
}
```

### 2. Breadth First Search

**Time complexity:** O(V+E)  
**Space complexity:** O(V)  
where V is the number of vertices and E is the number of edges.

#### Python
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        if not node:
            return None
            
        oldToNew = {}
        oldToNew[node] = Node(node.val)
        
        q = deque([node])
        
        while q:
            cur = q.popleft()
            
            for nei in cur.neighbors:
                if nei not in oldToNew:
                    oldToNew[nei] = Node(nei.val)
                    q.append(nei)
                    
                oldToNew[cur].neighbors.append(oldToNew[nei])
                
        return oldToNew[node]
```

#### JavaScript
```javascript
/**
 * // Definition for a Node.
 * function Node(val, neighbors) {
 *    this.val = val === undefined ? 0 : val;
 *    this.neighbors = neighbors === undefined ? [] : neighbors;
 * };
 */

/**
 * @param {Node} node
 * @return {Node}
 */
var cloneGraph = function(node) {
    if (!node) {
        return null;
    }
    
    const oldToNew = new Map();
    oldToNew.set(node, new Node(node.val));
    
    const queue = [node];
    
    while (queue.length > 0) {
        const curr = queue.shift();
        
        for (const nei of curr.neighbors) {
            if (!oldToNew.has(nei)) {
                oldToNew.set(nei, new Node(nei.val));
                queue.push(nei);
            }
            
            oldToNew.get(curr).neighbors.push(oldToNew.get(nei));
        }
    }
    
    return oldToNew.get(node);
};
```

#### Java
```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return null;
        }
        
        Map<Node, Node> oldToNew = new HashMap<>();
        Queue<Node> queue = new LinkedList<>();
        
        oldToNew.put(node, new Node(node.val));
        queue.offer(node);
        
        while (!queue.isEmpty()) {
            Node curr = queue.poll();
            
            for (Node nei : curr.neighbors) {
                if (!oldToNew.containsKey(nei)) {
                    oldToNew.put(nei, new Node(nei.val));
                    queue.offer(nei);
                }
                
                oldToNew.get(curr).neighbors.add(oldToNew.get(nei));
            }
        }
        
        return oldToNew.get(node);
    }
}
```
