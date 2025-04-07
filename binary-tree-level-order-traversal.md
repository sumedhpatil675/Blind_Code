# Binary Tree Level Order Traversal

## Problem Description
Given the `root` of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

## Approaches

### 1. Breadth-First Search (BFS) with Queue ✅
* Time complexity: O(n) where n is the number of nodes in the tree
* Space complexity: O(n) in the worst case (perfect binary tree's last level has n/2 nodes)

#### Python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        # Handle edge case: empty tree
        if not root:
            return []
        
        # Initialize result list and queue
        result = []
        queue = deque([root])
        
        # BFS traversal
        while queue:
            # Get the number of nodes at current level
            level_size = len(queue)
            level_nodes = []
            
            # Process all nodes at current level
            for _ in range(level_size):
                # Dequeue a node
                node = queue.popleft()
                level_nodes.append(node.val)
                
                # Enqueue its children
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            
            # Add current level to result
            result.append(level_nodes)
        
        return result
```

#### JavaScript
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var levelOrder = function(root) {
    // Handle edge case: empty tree
    if (root === null) {
        return [];
    }
    
    // Initialize result array and queue
    const result = [];
    const queue = [root];
    
    // BFS traversal
    while (queue.length > 0) {
        // Get the number of nodes at current level
        const levelSize = queue.length;
        const levelNodes = [];
        
        // Process all nodes at current level
        for (let i = 0; i < levelSize; i++) {
            // Dequeue a node
            const node = queue.shift();
            levelNodes.push(node.val);
            
            // Enqueue its children
            if (node.left !== null) {
                queue.push(node.left);
            }
            if (node.right !== null) {
                queue.push(node.right);
            }
        }
        
        // Add current level to result
        result.push(levelNodes);
    }
    
    return result;
};
```

#### Java
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        // Initialize result list
        List<List<Integer>> result = new ArrayList<>();
        
        // Handle edge case: empty tree
        if (root == null) {
            return result;
        }
        
        // Initialize queue for BFS
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        // BFS traversal
        while (!queue.isEmpty()) {
            // Get the number of nodes at current level
            int levelSize = queue.size();
            List<Integer> levelNodes = new ArrayList<>();
            
            // Process all nodes at current level
            for (int i = 0; i < levelSize; i++) {
                // Dequeue a node
                TreeNode node = queue.poll();
                levelNodes.add(node.val);
                
                // Enqueue its children
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            
            // Add current level to result
            result.add(levelNodes);
        }
        
        return result;
    }
}
```

### 2. Recursive Depth-First Search (DFS)
* Time complexity: O(n) where n is the number of nodes in the tree
* Space complexity: O(h) for the recursion stack, where h is the height of the tree (worst case O(n) for skewed tree)

#### Python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        # Initialize result list
        result = []
        
        # Helper function for recursive DFS
        def dfs(node, level):
            # Base case: null node
            if not node:
                return
            
            # If this is the first node at this level, create a new list for the level
            if len(result) <= level:
                result.append([])
            
            # Add the current node's value to its level's list
            result[level].append(node.val)
            
            # Recursively process children at the next level
            dfs(node.left, level + 1)
            dfs(node.right, level + 1)
        
        # Start DFS from root at level 0
        dfs(root, 0)
        return result
```

#### JavaScript
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var levelOrder = function(root) {
    // Initialize result array
    const result = [];
    
    // Helper function for recursive DFS
    function dfs(node, level) {
        // Base case: null node
        if (node === null) {
            return;
        }
        
        // If this is the first node at this level, create a new array for the level
        if (result.length <= level) {
            result.push([]);
        }
        
        // Add the current node's value to its level's array
        result[level].push(node.val);
        
        // Recursively process children at the next level
        dfs(node.left, level + 1);
        dfs(node.right, level + 1);
    }
    
    // Start DFS from root at level 0
    dfs(root, 0);
    return result;
};
```

#### Java
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        // Initialize result list
        List<List<Integer>> result = new ArrayList<>();
        
        // Call helper function
        dfs(root, 0, result);
        return result;
    }
    
    private void dfs(TreeNode node, int level, List<List<Integer>> result) {
        // Base case: null node
        if (node == null) {
            return;
        }
        
        // If this is the first node at this level, create a new list for the level
        if (result.size() <= level) {
            result.add(new ArrayList<>());
        }
        
        // Add the current node's value to its level's list
        result.get(level).add(node.val);
        
        // Recursively process children at the next level
        dfs(node.left, level + 1, result);
        dfs(node.right, level + 1, result);
    }
}
```

### 3. Iterative DFS with Level Tracking
* Time complexity: O(n) where n is the number of nodes in the tree
* Space complexity: O(h) where h is the height of the tree (worst case O(n) for skewed tree)

#### Python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        # Handle edge case: empty tree
        if not root:
            return []
        
        # Initialize result list
        result = []
        
        # DFS with stack (node, level) pairs
        stack = [(root, 0)]
        
        while stack:
            node, level = stack.pop()
            
            # If this is the first node at this level, create a new list for the level
            if len(result) <= level:
                result.append([])
            
            # Add the current node's value to its level's list
            result[level].append(node.val)
            
            # Push right child first so that left child is processed first (for level order)
            if node.right:
                stack.append((node.right, level + 1))
            if node.left:
                stack.append((node.left, level + 1))
        
        return result
```

#### JavaScript
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var levelOrder = function(root) {
    // Handle edge case: empty tree
    if (root === null) {
        return [];
    }
    
    // Initialize result array
    const result = [];
    
    // DFS with stack (node, level) pairs
    const stack = [[root, 0]];
    
    while (stack.length > 0) {
        const [node, level] = stack.pop();
        
        // If this is the first node at this level, create a new array for the level
        if (result.length <= level) {
            result.push([]);
        }
        
        // Add the current node's value to its level's array
        result[level].push(node.val);
        
        // Push right child first so that left child is processed first (for level order)
        if (node.right !== null) {
            stack.push([node.right, level + 1]);
        }
        if (node.left !== null) {
            stack.push([node.left, level + 1]);
        }
    }
    
    return result;
};
```

#### Java
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        // Initialize result list
        List<List<Integer>> result = new ArrayList<>();
        
        // Handle edge case: empty tree
        if (root == null) {
            return result;
        }
        
        // DFS with stack (node, level) pairs
        Stack<Pair<TreeNode, Integer>> stack = new Stack<>();
        stack.push(new Pair<>(root, 0));
        
        while (!stack.isEmpty()) {
            Pair<TreeNode, Integer> pair = stack.pop();
            TreeNode node = pair.getKey();
            int level = pair.getValue();
            
            // If this is the first node at this level, create a new list for the level
            if (result.size() <= level) {
                result.add(new ArrayList<>());
            }
            
            // Add the current node's value to its level's list
            result.get(level).add(node.val);
            
            // Push right child first so that left child is processed first (for level order)
            if (node.right != null) {
                stack.push(new Pair<>(node.right, level + 1));
            }
            if (node.left != null) {
                stack.push(new Pair<>(node.left, level + 1));
            }
        }
        
        return result;
    }
}
```
