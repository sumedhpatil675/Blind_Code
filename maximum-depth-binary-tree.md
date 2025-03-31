# Maximum Depth of Binary Tree

## Problem Description
Given the `root` of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

## Approaches

### 1. Recursive Depth-First Search (DFS)
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
    def maxDepth(self, root: TreeNode) -> int:
        # Base case: empty tree has depth 0
        if not root:
            return 0
        
        # Recursive case: depth is 1 (for current node) 
        # plus the maximum depth of the left and right subtrees
        left_depth = self.maxDepth(root.left)
        right_depth = self.maxDepth(root.right)
        
        return 1 + max(left_depth, right_depth)
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
 * @return {number}
 */
var maxDepth = function(root) {
    // Base case: empty tree has depth 0
    if (root === null) {
        return 0;
    }
    
    // Recursive case: depth is 1 (for current node) 
    // plus the maximum depth of the left and right subtrees
    const leftDepth = maxDepth(root.left);
    const rightDepth = maxDepth(root.right);
    
    return 1 + Math.max(leftDepth, rightDepth);
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
    public int maxDepth(TreeNode root) {
        // Base case: empty tree has depth 0
        if (root == null) {
            return 0;
        }
        
        // Recursive case: depth is 1 (for current node) 
        // plus the maximum depth of the left and right subtrees
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        
        return 1 + Math.max(leftDepth, rightDepth);
    }
}
```

### 2. Iterative Breadth-First Search (BFS) using Queue
* Time complexity: O(n) where n is the number of nodes in the tree
* Space complexity: O(w) where w is the maximum width of the tree (worst case O(n/2) for perfect binary tree)

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
    def maxDepth(self, root: TreeNode) -> int:
        # Handle empty tree
        if not root:
            return 0
        
        # BFS with queue
        queue = deque([root])
        depth = 0
        
        while queue:
            # Process all nodes at current level
            for _ in range(len(queue)):
                node = queue.popleft()
                
                # Add children to the queue
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            
            # Increment depth after processing all nodes at current level
            depth += 1
        
        return depth
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
 * @return {number}
 */
var maxDepth = function(root) {
    // Handle empty tree
    if (root === null) {
        return 0;
    }
    
    // BFS with queue
    const queue = [root];
    let depth = 0;
    
    while (queue.length > 0) {
        // Process all nodes at current level
        const levelSize = queue.length;
        
        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift();
            
            // Add children to the queue
            if (node.left !== null) {
                queue.push(node.left);
            }
            if (node.right !== null) {
                queue.push(node.right);
            }
        }
        
        // Increment depth after processing all nodes at current level
        depth++;
    }
    
    return depth;
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
    public int maxDepth(TreeNode root) {
        // Handle empty tree
        if (root == null) {
            return 0;
        }
        
        // BFS with queue
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int depth = 0;
        
        while (!queue.isEmpty()) {
            // Process all nodes at current level
            int levelSize = queue.size();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                
                // Add children to the queue
                if (node.left != null) {
                    queue.add(node.left);
                }
                if (node.right != null) {
                    queue.add(node.right);
                }
            }
            
            // Increment depth after processing all nodes at current level
            depth++;
        }
        
        return depth;
    }
}
```

### 3. Iterative Depth-First Search (DFS) using Stack
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
    def maxDepth(self, root: TreeNode) -> int:
        # Handle empty tree
        if not root:
            return 0
        
        # DFS with stack
        stack = [(root, 1)]  # Pairs of (node, depth)
        max_depth = 0
        
        while stack:
            node, depth = stack.pop()
            
            # Update max depth
            max_depth = max(max_depth, depth)
            
            # Add children to the stack
            if node.right:
                stack.append((node.right, depth + 1))
            if node.left:
                stack.append((node.left, depth + 1))
        
        return max_depth
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
 * @return {number}
 */
var maxDepth = function(root) {
    // Handle empty tree
    if (root === null) {
        return 0;
    }
    
    // DFS with stack
    const stack = [[root, 1]];  // Pairs of [node, depth]
    let maxDepth = 0;
    
    while (stack.length > 0) {
        const [node, depth] = stack.pop();
        
        // Update max depth
        maxDepth = Math.max(maxDepth, depth);
        
        // Add children to the stack
        if (node.right !== null) {
            stack.push([node.right, depth + 1]);
        }
        if (node.left !== null) {
            stack.push([node.left, depth + 1]);
        }
    }
    
    return maxDepth;
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
    public int maxDepth(TreeNode root) {
        // Handle empty tree
        if (root == null) {
            return 0;
        }
        
        // DFS with stack using a helper class to store node and depth
        Stack<NodeDepth> stack = new Stack<>();
        stack.push(new NodeDepth(root, 1));
        int maxDepth = 0;
        
        while (!stack.isEmpty()) {
            NodeDepth current = stack.pop();
            TreeNode node = current.node;
            int depth = current.depth;
            
            // Update max depth
            maxDepth = Math.max(maxDepth, depth);
            
            // Add children to the stack
            if (node.right != null) {
                stack.push(new NodeDepth(node.right, depth + 1));
            }
            if (node.left != null) {
                stack.push(new NodeDepth(node.left, depth + 1));
            }
        }
        
        return maxDepth;
    }
    
    // Helper class to store a node and its depth
    private class NodeDepth {
        TreeNode node;
        int depth;
        
        NodeDepth(TreeNode node, int depth) {
            this.node = node;
            this.depth = depth;
        }
    }
}
```