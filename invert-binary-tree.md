# Invert Binary Tree

## Problem Description
Given the `root` of a binary tree, invert the tree, and return its root.

To invert a binary tree, you need to swap each node's left and right children.

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
    def invertTree(self, root: TreeNode) -> TreeNode:
        # Base case: if the node is None, return None
        if not root:
            return None
        
        # Swap the left and right children
        root.left, root.right = root.right, root.left
        
        # Recursively invert the left and right subtrees
        self.invertTree(root.left)
        self.invertTree(root.right)
        
        # Return the root node
        return root
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
 * @return {TreeNode}
 */
var invertTree = function(root) {
    // Base case: if the node is null, return null
    if (root === null) {
        return null;
    }
    
    // Swap the left and right children
    const temp = root.left;
    root.left = root.right;
    root.right = temp;
    
    // Recursively invert the left and right subtrees
    invertTree(root.left);
    invertTree(root.right);
    
    // Return the root node
    return root;
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
    public TreeNode invertTree(TreeNode root) {
        // Base case: if the node is null, return null
        if (root == null) {
            return null;
        }
        
        // Swap the left and right children
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        
        // Recursively invert the left and right subtrees
        invertTree(root.left);
        invertTree(root.right);
        
        // Return the root node
        return root;
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
    def invertTree(self, root: TreeNode) -> TreeNode:
        # If the tree is empty, return None
        if not root:
            return None
        
        # Create a queue for BFS
        queue = deque([root])
        
        # Process nodes in the queue
        while queue:
            # Get the current node
            current = queue.popleft()
            
            # Swap its left and right children
            current.left, current.right = current.right, current.left
            
            # Add non-null children to the queue
            if current.left:
                queue.append(current.left)
            if current.right:
                queue.append(current.right)
        
        # Return the root node
        return root
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
 * @return {TreeNode}
 */
var invertTree = function(root) {
    // If the tree is empty, return null
    if (root === null) {
        return null;
    }
    
    // Create a queue for BFS
    const queue = [root];
    
    // Process nodes in the queue
    while (queue.length > 0) {
        // Get the current node
        const current = queue.shift();
        
        // Swap its left and right children
        const temp = current.left;
        current.left = current.right;
        current.right = temp;
        
        // Add non-null children to the queue
        if (current.left !== null) {
            queue.push(current.left);
        }
        if (current.right !== null) {
            queue.push(current.right);
        }
    }
    
    // Return the root node
    return root;
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
    public TreeNode invertTree(TreeNode root) {
        // If the tree is empty, return null
        if (root == null) {
            return null;
        }
        
        // Create a queue for BFS
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        // Process nodes in the queue
        while (!queue.isEmpty()) {
            // Get the current node
            TreeNode current = queue.poll();
            
            // Swap its left and right children
            TreeNode temp = current.left;
            current.left = current.right;
            current.right = temp;
            
            // Add non-null children to the queue
            if (current.left != null) {
                queue.add(current.left);
            }
            if (current.right != null) {
                queue.add(current.right);
            }
        }
        
        // Return the root node
        return root;
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
    def invertTree(self, root: TreeNode) -> TreeNode:
        # If the tree is empty, return None
        if not root:
            return None
        
        # Create a stack for DFS
        stack = [root]
        
        # Process nodes in the stack
        while stack:
            # Get the current node
            current = stack.pop()
            
            # Swap its left and right children
            current.left, current.right = current.right, current.left
            
            # Add non-null children to the stack
            if current.right:
                stack.append(current.right)
            if current.left:
                stack.append(current.left)
        
        # Return the root node
        return root
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
 * @return {TreeNode}
 */
var invertTree = function(root) {
    // If the tree is empty, return null
    if (root === null) {
        return null;
    }
    
    // Create a stack for DFS
    const stack = [root];
    
    // Process nodes in the stack
    while (stack.length > 0) {
        // Get the current node
        const current = stack.pop();
        
        // Swap its left and right children
        const temp = current.left;
        current.left = current.right;
        current.right = temp;
        
        // Add non-null children to the stack
        if (current.right !== null) {
            stack.push(current.right);
        }
        if (current.left !== null) {
            stack.push(current.left);
        }
    }
    
    // Return the root node
    return root;
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
    public TreeNode invertTree(TreeNode root) {
        // If the tree is empty, return null
        if (root == null) {
            return null;
        }
        
        // Create a stack for DFS
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        
        // Process nodes in the stack
        while (!stack.isEmpty()) {
            // Get the current node
            TreeNode current = stack.pop();
            
            // Swap its left and right children
            TreeNode temp = current.left;
            current.left = current.right;
            current.right = temp;
            
            // Add non-null children to the stack
            if (current.right != null) {
                stack.push(current.right);
            }
            if (current.left != null) {
                stack.push(current.left);
            }
        }
        
        // Return the root node
        return root;
    }
}
```