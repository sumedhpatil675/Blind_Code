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

```javascript
/**
 * Determines if all courses can be finished given their prerequisites.
 * @param {number} numCourses - The total number of courses
 * @param {number[][]} prerequisites - Array of [course, prerequisite] pairs
 * @return {boolean} - Whether all courses can be completed
 */
function canFinish(numCourses, prerequisites) {
    // Create adjacency list for each course
    const preMap = {};
    for (let i = 0; i < numCourses; i++) {
        preMap[i] = [];
    }
    
    // Populate the adjacency list
    for (const [crs, pre] of prerequisites) {
        preMap[crs].push(pre);
    }
    
    // visitSet tracks courses in the current DFS path
    const visitSet = new Set();
    
    /**
     * Depth-first search to check if course can be completed
     * @param {number} crs - The course to check
     * @return {boolean} - Whether the course can be completed
     */
    function dfs(crs) {
        // If course is in current path, we found a cycle
        if (visitSet.has(crs)) {
            return false;
        }
        
        // If course has no prerequisites, it can be completed
        if (preMap[crs].length === 0) {
            return true;
        }
        
        // Add course to current path
        visitSet.add(crs);
        
        // Check all prerequisites recursively
        for (const pre of preMap[crs]) {
            if (!dfs(pre)) {
                return false;
            }
        }
        
        // Remove course from current path
        visitSet.delete(crs);
        
        // Mark as "visited" by emptying prerequisites
        preMap[crs] = [];
        
        return true;
    }
    
    // Try to complete each course
    for (let crs = 0; crs < numCourses; crs++) {
        if (!dfs(crs)) {
            return false;
        }
    }
    
    return true;
}
```

## Java Implementation

```java
import java.util.*;

/**
 * Solution for the Course Schedule problem
 */
public class CourseSchedule {
    
    /**
     * Determines if all courses can be finished given their prerequisites.
     * @param numCourses The total number of courses
     * @param prerequisites Array of [course, prerequisite] pairs
     * @return Whether all courses can be completed
     */
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Create adjacency list for prerequisites
        Map<Integer, List<Integer>> preMap = new HashMap<>();
        
        // Initialize empty lists for all courses
        for (int i = 0; i < numCourses; i++) {
            preMap.put(i, new ArrayList<>());
        }
        
        // Fill the adjacency list
        for (int[] prereq : prerequisites) {
            int course = prereq[0];
            int prerequisite = prereq[1];
            preMap.get(course).add(prerequisite);
        }
        
        // Set to track courses in current DFS path
        Set<Integer> visitSet = new HashSet<>();
        
        // Try to complete all courses
        for (int course = 0; course < numCourses; course++) {
            if (!dfs(course, preMap, visitSet)) {
                return false;
            }
        }
        
        return true;
    }
    
    /**
     * Depth-first search to check if course can be completed
     * @param course The course to check
     * @param preMap Map of course to prerequisites
     * @param visitSet Set of courses in current path
     * @return Whether the course can be completed
     */
    private boolean dfs(int course, Map<Integer, List<Integer>> preMap, Set<Integer> visitSet) {
        // If course is in current path, we found a cycle
        if (visitSet.contains(course)) {
            return false;
        }
        
        // If course has no prerequisites, it can be completed
        if (preMap.get(course).isEmpty()) {
            return true;
        }
        
        // Add course to current path
        visitSet.add(course);
        
        // Check all prerequisites recursively
        for (int prereq : preMap.get(course)) {
            if (!dfs(prereq, preMap, visitSet)) {
                return false;
            }
        }
        
        // Remove course from current path
        visitSet.remove(course);
        
        // Mark as "visited" by emptying prerequisites
        preMap.get(course).clear();
        
        return true;
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
