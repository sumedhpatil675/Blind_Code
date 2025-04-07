# Same Tree

## Problem Description
Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

## Approaches

### 1. Recursive Depth-First Search (DFS) ✅✅✅

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
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        # If both nodes are None, they are the same
        if not p and not q:
            return True
        
        # If one is None and the other is not, they are different
        if not p or not q:
            return False
        
        # If the values are different, they are different trees
        if p.val != q.val:
            return False
        
        # Recursively check left and right subtrees
        return (self.isSameTree(p.left, q.left) and 
                self.isSameTree(p.right, q.right))
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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function(p, q) {
    // If both nodes are null, they are the same
    if (p === null && q === null) {
        return true;
    }
    
    // If one is null and the other is not, they are different
    if (p === null || q === null) {
        return false;
    }
    
    // If the values are different, they are different trees
    if (p.val !== q.val) {
        return false;
    }
    
    // Recursively check left and right subtrees
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // If both nodes are null, they are the same
        if (p == null && q == null) {
            return true;
        }
        
        // If one is null and the other is not, they are different
        if (p == null || q == null) {
            return false;
        }
        
        // If the values are different, they are different trees
        if (p.val != q.val) {
            return false;
        }
        
        // Recursively check left and right subtrees
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
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
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        # Initialize a queue with both root nodes
        queue = deque([(p, q)])
        
        while queue:
            node1, node2 = queue.popleft()
            
            # If both nodes are None, continue
            if not node1 and not node2:
                continue
            
            # If one is None and the other is not, they are different
            if not node1 or not node2:
                return False
            
            # If the values are different, they are different trees
            if node1.val != node2.val:
                return False
            
            # Add left and right children pairs to the queue
            queue.append((node1.left, node2.left))
            queue.append((node1.right, node2.right))
        
        return True
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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function(p, q) {
    // Initialize a queue with both root nodes
    const queue = [[p, q]];
    
    while (queue.length > 0) {
        const [node1, node2] = queue.shift();
        
        // If both nodes are null, continue
        if (node1 === null && node2 === null) {
            continue;
        }
        
        // If one is null and the other is not, they are different
        if (node1 === null || node2 === null) {
            return false;
        }
        
        // If the values are different, they are different trees
        if (node1.val !== node2.val) {
            return false;
        }
        
        // Add left and right children pairs to the queue
        queue.push([node1.left, node2.left]);
        queue.push([node1.right, node2.right]);
    }
    
    return true;
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // Initialize a queue with both root nodes
        Queue<TreeNode[]> queue = new LinkedList<>();
        queue.add(new TreeNode[]{p, q});
        
        while (!queue.isEmpty()) {
            TreeNode[] nodes = queue.poll();
            TreeNode node1 = nodes[0];
            TreeNode node2 = nodes[1];
            
            // If both nodes are null, continue
            if (node1 == null && node2 == null) {
                continue;
            }
            
            // If one is null and the other is not, they are different
            if (node1 == null || node2 == null) {
                return false;
            }
            
            // If the values are different, they are different trees
            if (node1.val != node2.val) {
                return false;
            }
            
            // Add left and right children pairs to the queue
            queue.add(new TreeNode[]{node1.left, node2.left});
            queue.add(new TreeNode[]{node1.right, node2.right});
        }
        
        return true;
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
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        # Initialize a stack with both root nodes
        stack = [(p, q)]
        
        while stack:
            node1, node2 = stack.pop()
            
            # If both nodes are None, continue
            if not node1 and not node2:
                continue
            
            # If one is None and the other is not, they are different
            if not node1 or not node2:
                return False
            
            # If the values are different, they are different trees
            if node1.val != node2.val:
                return False
            
            # Add right and left children pairs to the stack
            # (right first so left is processed first due to LIFO)
            stack.append((node1.right, node2.right))
            stack.append((node1.left, node2.left))
        
        return True
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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function(p, q) {
    // Initialize a stack with both root nodes
    const stack = [[p, q]];
    
    while (stack.length > 0) {
        const [node1, node2] = stack.pop();
        
        // If both nodes are null, continue
        if (node1 === null && node2 === null) {
            continue;
        }
        
        // If one is null and the other is not, they are different
        if (node1 === null || node2 === null) {
            return false;
        }
        
        // If the values are different, they are different trees
        if (node1.val !== node2.val) {
            return false;
        }
        
        // Add right and left children pairs to the stack
        // (right first so left is processed first due to LIFO)
        stack.push([node1.right, node2.right]);
        stack.push([node1.left, node2.left]);
    }
    
    return true;
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // Initialize a stack with both root nodes
        Stack<TreeNode[]> stack = new Stack<>();
        stack.push(new TreeNode[]{p, q});
        
        while (!stack.isEmpty()) {
            TreeNode[] nodes = stack.pop();
            TreeNode node1 = nodes[0];
            TreeNode node2 = nodes[1];
            
            // If both nodes are null, continue
            if (node1 == null && node2 == null) {
                continue;
            }
            
            // If one is null and the other is not, they are different
            if (node1 == null || node2 == null) {
                return false;
            }
            
            // If the values are different, they are different trees
            if (node1.val != node2.val) {
                return false;
            }
            
            // Add right and left children pairs to the stack
            // (right first so left is processed first due to LIFO)
            stack.push(new TreeNode[]{node1.right, node2.right});
            stack.push(new TreeNode[]{node1.left, node2.left});
        }
        
        return true;
    }
}
```
