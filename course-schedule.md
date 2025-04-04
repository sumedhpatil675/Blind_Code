# Course Schedule

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.

Return true if you can finish all courses. Otherwise, return false.

## Approaches

### 1. Cycle Detection (DFS)

**Time complexity:** O(V+E)  
**Space complexity:** O(V+E)  
where V is the number of courses and E is the number of prerequisites.

#### Python
```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # Map each course to its prerequisites
        preMap = {i: [] for i in range(numCourses)}
        for crs, pre in prerequisites:
            preMap[crs].append(pre)
            
        # Store all courses along the current DFS path
        visiting = set()
        
        def dfs(crs):
            if crs in visiting:
                # Cycle detected
                return False
                
            if preMap[crs] == []:
                return True
                
            visiting.add(crs)
            
            for pre in preMap[crs]:
                if not dfs(pre):
                    return False
                    
            visiting.remove(crs)
            preMap[crs] = []
            
            return True
            
        for c in range(numCourses):
            if not dfs(c):
                return False
                
        return True
```

# Course Schedule Problem Explanation

## The Problem

The Course Schedule problem asks whether it's possible to finish all courses, given the prerequisites for each course. This is essentially checking for cycles in a directed graph, where:
- Each course is a node
- Each prerequisite is a directed edge

## The Solution

```python
def canFinish(numCourses, prerequisites):
    preMap = {i:[] for i in range(numCourses)}
    for crs, pre in prerequisites:
        preMap[crs].append(pre)
    
    # visitSet = all courses along the curr DFS path
    visitSet = set()
    
    def dfs(crs):
        if crs in visitSet:
            return False
        if preMap[crs] == []:
            return True
            
        visitSet.add(crs)
        for pre in preMap[crs]:
            if not dfs(pre): return False
        visitSet.remove(crs)
        preMap[crs] = []
        return True
    
    for crs in range(numCourses):
        if not dfs(crs): return False
    return True
```

## Example with Cycle

Let's trace through an example with a cycle:
- `numCourses = 3`
- `prerequisites = [[0,1], [1,2], [2,0]]` (0 depends on 1, 1 depends on 2, 2 depends on 0)

### Initial State

```
preMap = {
    0: [1],  # To take course 0, you need course 1
    1: [2],  # To take course 1, you need course 2
    2: [0]   # To take course 2, you need course 0
}

visitSet = {}  # Empty set
```

### DFS Execution Trace

```
┌─────────────────────────────────────────────────────────────┐
│ Function Call Stack      │ visitSet      │ Action           │
├────────────────────────────────────────────────────────────-┤
│ dfs(0)                   │ {}            │ Check for cycle  │
│ dfs(0)                   │ {0}           │ Add 0 to path    │
│ dfs(0) → dfs(1)          │ {0}           │ Check prereq 1   │
│ dfs(0) → dfs(1)          │ {0,1}         │ Add 1 to path    │
│ dfs(0) → dfs(1) → dfs(2) │ {0,1}         │ Check prereq 2   │
│ dfs(0) → dfs(1) → dfs(2) │ {0,1,2}       │ Add 2 to path    │
│ dfs(0) → dfs(1) → dfs(2) │ {0,1,2}       │ Check prereq 0   │
│ → dfs(0)                 │               │                  │
└─────────────────────────────────────────────────────────────┘
```

At this point, DFS(0) is called again, but 0 is already in the visitSet, so we detect a cycle and return False.

### Visualization of the Cycle

```
    ┌───┐
    │ 0 │
    └───┘
     ▲ │
     │ ▼
┌───┐   ┌───┐
│ 2 │◄──┤ 1 │
└───┘   └───┘
```

## Example Without Cycle

Let's try another example:
- `numCourses = 3`
- `prerequisites = [[0,1], [1,2]]` (0 depends on 1, 1 depends on 2)

### Initial State

```
preMap = {
    0: [1],  # To take course 0, you need course 1
    1: [2],  # To take course 1, you need course 2
    2: []    # No prerequisites for course 2
}

visitSet = {}  # Empty set
```

### DFS Execution Trace

```
┌─────────────────────────────────────────────────────────────────────────┐
│ Function Call Stack      │ visitSet      │ preMap                │ Action               │
├────────────────────────────────────────────────────────────────────────-┤
│ dfs(0)                   │ {}            │ {0:[1], 1:[2], 2:[]}  │ Check for cycle      │
│ dfs(0)                   │ {0}           │ {0:[1], 1:[2], 2:[]}  │ Add 0 to path        │
│ dfs(0) → dfs(1)          │ {0}           │ {0:[1], 1:[2], 2:[]}  │ Check prereq 1       │
│ dfs(0) → dfs(1)          │ {0,1}         │ {0:[1], 1:[2], 2:[]}  │ Add 1 to path        │
│ dfs(0) → dfs(1) → dfs(2) │ {0,1}         │ {0:[1], 1:[2], 2:[]}  │ Check prereq 2       │
│ dfs(0) → dfs(1) → dfs(2) │ {0,1,2}       │ {0:[1], 1:[2], 2:[]}  │ Add 2 to path        │
│ dfs(0) → dfs(1) → dfs(2) │ {0,1,2}       │ {0:[1], 1:[2], 2:[]}  │ Empty prereq list    │
│ dfs(0) → dfs(1) → dfs(2) │ {0,1,2}       │ {0:[1], 1:[2], 2:[]}  │ Return True          │
│ dfs(0) → dfs(1)          │ {0,1}         │ {0:[1], 1:[], 2:[]}   │ Clear 2's prereqs    │
│ dfs(0) → dfs(1)          │ {0,1}         │ {0:[1], 1:[], 2:[]}   │ Return True          │
│ dfs(0)                   │ {0}           │ {0:[], 1:[], 2:[]}    │ Clear 1's prereqs    │
│ dfs(0)                   │ {0}           │ {0:[], 1:[], 2:[]}    │ Return True          │
│ Main function            │ {}            │ {0:[], 1:[], 2:[]}    │ Clear 0's prereqs    │
└─────────────────────────────────────────────────────────────────────────┘
```

The DFS for each course returns True, so all courses can be completed.

### Visualization of the Valid Course Order

```
┌───┐   ┌───┐   ┌───┐
│ 0 │◄──┤ 1 │◄──┤ 2 │
└───┘   └───┘   └───┘

Take in order: 2 → 1 → 0
```

## Understanding the Key Components

1. **preMap**: Adjacency list representing prerequisite relationships
   - Maps each course to the list of courses it depends on

2. **visitSet**: Tracks the current DFS path
   - Used to detect cycles (if we encounter a course already in the path)

3. **DFS function**:
   - If a course is in visitSet → Cycle detected → Return False
   - If a course has no prerequisites → Can be completed → Return True
   - Otherwise → Explore prerequisites recursively
   - After exploration → Mark as completed by emptying its prerequisite list

4. **Optimization**:
   - After successfully processing a course, we empty its prerequisite list
   - This acts as memoization, avoiding repeated computations

## Time and Space Complexity

- **Time Complexity**: O(V + E) where V is the number of courses and E is the number of prerequisite relationships
- **Space Complexity**: O(V + E) for the adjacency list and the recursion stack

#### JavaScript
```javascript
/**
 * @param {number} numCourses
 * @param {number[][]} prerequisites
 * @return {boolean}
 */
var canFinish = function(numCourses, prerequisites) {
    // Map each course to its prerequisites
    const preMap = new Map();
    for (let i = 0; i < numCourses; i++) {
        preMap.set(i, []);
    }
    
    for (const [course, prereq] of prerequisites) {
        preMap.get(course).push(prereq);
    }
    
    // Set to keep track of courses in the current DFS path
    const visiting = new Set();
    // Set to keep track of already verified courses
    const completed = new Set();
    
    function dfs(course) {
        if (visiting.has(course)) {
            // Cycle detected
            return false;
        }
        
        if (completed.has(course)) {
            return true;
        }
        
        if (preMap.get(course).length === 0) {
            completed.add(course);
            return true;
        }
        
        visiting.add(course);
        
        for (const prereq of preMap.get(course)) {
            if (!dfs(prereq)) {
                return false;
            }
        }
        
        visiting.delete(course);
        completed.add(course);
        preMap.set(course, []);
        
        return true;
    }
    
    for (let i = 0; i < numCourses; i++) {
        if (!dfs(i)) {
            return false;
        }
    }
    
    return true;
};
```

#### Java
```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Map each course to its prerequisites
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            adj.add(new ArrayList<>());
        }
        
        for (int[] prereq : prerequisites) {
            adj.get(prereq[0]).add(prereq[1]);
        }
        
        // 0 = unvisited, 1 = in current path, 2 = processed
        int[] visited = new int[numCourses];
        
        for (int i = 0; i < numCourses; i++) {
            if (visited[i] == 0) {
                if (hasCycle(i, adj, visited)) {
                    return false;
                }
            }
        }
        
        return true;
    }
    
    private boolean hasCycle(int course, List<List<Integer>> adj, int[] visited) {
        // If the course is in the current DFS path, we found a cycle
        if (visited[course] == 1) {
            return true;
        }
        
        // If the course has been processed already, we know it's safe
        if (visited[course] == 2) {
            return false;
        }
        
        // Mark the course as being in the current DFS path
        visited[course] = 1;
        
        // Check all prerequisites
        for (int prereq : adj.get(course)) {
            if (hasCycle(prereq, adj, visited)) {
                return true;
            }
        }
        
        // Mark the course as processed (no cycle found)
        visited[course] = 2;
        return false;
    }
}
```

### 2. Topological Sort (Kahn's Algorithm)

**Time complexity:** O(V+E)  
**Space complexity:** O(V+E)  
where V is the number of courses and E is the number of prerequisites.

#### Python
```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # Create adjacency list
        adj = [[] for i in range(numCourses)]
        # Calculate in-degrees for each course
        indegree = [0] * numCourses
        
        for crs, pre in prerequisites:
            adj[pre].append(crs)
            indegree[crs] += 1
            
        # Start with courses that have no prerequisites
        q = deque()
        for n in range(numCourses):
            if indegree[n] == 0:
                q.append(n)
                
        finish = 0
        while q:
            node = q.popleft()
            finish += 1
            
            for nei in adj[node]:
                indegree[nei] -= 1
                if indegree[nei] == 0:
                    q.append(nei)
                    
        return finish == numCourses
```

#### JavaScript
```javascript
/**
 * @param {number} numCourses
 * @param {number[][]} prerequisites
 * @return {boolean}
 */
var canFinish = function(numCourses, prerequisites) {
    // Create adjacency list
    const adj = Array(numCourses).fill().map(() => []);
    // Calculate in-degrees for each course
    const indegree = Array(numCourses).fill(0);
    
    for (const [course, prereq] of prerequisites) {
        adj[prereq].push(course);
        indegree[course]++;
    }
    
    // Start with courses that have no prerequisites
    const queue = [];
    for (let i = 0; i < numCourses; i++) {
        if (indegree[i] === 0) {
            queue.push(i);
        }
    }
    
    let coursesFinished = 0;
    
    while (queue.length > 0) {
        const current = queue.shift();
        coursesFinished++;
        
        for (const next of adj[current]) {
            indegree[next]--;
            if (indegree[next] === 0) {
                queue.push(next);
            }
        }
    }
    
    return coursesFinished === numCourses;
};
```

#### Java
```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Create adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            adj.add(new ArrayList<>());
        }
        
        // Calculate in-degrees for each course
        int[] indegree = new int[numCourses];
        
        for (int[] prereq : prerequisites) {
            adj.get(prereq[1]).add(prereq[0]);
            indegree[prereq[0]]++;
        }
        
        // Start with courses that have no prerequisites
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        int coursesFinished = 0;
        
        while (!queue.isEmpty()) {
            int current = queue.poll();
            coursesFinished++;
            
            for (int next : adj.get(current)) {
                indegree[next]--;
                if (indegree[next] == 0) {
                    queue.offer(next);
                }
            }
        }
        
        return coursesFinished == numCourses;
    }
}
```
