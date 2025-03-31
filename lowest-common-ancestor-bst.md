# Lowest Common Ancestor of a Binary Search Tree

## Problem Description
Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: "The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in the tree that has both `p` and `q` as descendants (where we allow a node to be a descendant of itself)."

## Approaches

### 1. Recursive Approach
* Time complexity: O(h) where h is the height of the tree (worst case O(n) for skewed tree)
* Space complexity: O(h) for the recursion stack (worst case O(n) for skewed tree)

#### Python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # Get values for convenience
        root_val = root.val
        p_val = p.val
        q_val = q.val
        
        # If both p and q are greater than root, LCA must be in right subtree
        if p_val > root_val and q_val > root_val:
            return self.lowestCommonAncestor(root.right, p, q)
        
        # If both p and q are less than root, LCA must be in left subtree
        if p_val < root_val and q_val < root_val:
            return self.lowestCommonAncestor(root.left, p, q)
        
        # If p and q are on different sides of root (or one of them is the root)
        # then the root is their LCA
        return root
```

#### JavaScript
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function(root, p, q) {
    // Get values for convenience
    const rootVal = root.val;
    const pVal = p.val;
    const qVal = q.val;
    
    // If both p and q are greater than root, LCA must be in right subtree
    if (pVal > rootVal && qVal > rootVal) {
        return lowestCommonAncestor(root.right, p, q);
    }
    
    // If both p and q are less than root, LCA must be in left subtree
    if (pVal < rootVal && qVal < rootVal) {
        return lowestCommonAncestor(root.left, p, q);
    }
    
    // If p and q are on different sides of root (or one of them is the root)
    // then the root is their LCA
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
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Get values for convenience
        int rootVal = root.val;
        int pVal = p.val;
        int qVal = q.val;
        
        // If both p and q are greater than root, LCA must be in right subtree
        if (pVal > rootVal && qVal > rootVal) {
            return lowestCommonAncestor(root.right, p, q);
        }
        
        // If both p and q are less than root, LCA must be in left subtree
        if (pVal < rootVal && qVal < rootVal) {
            return lowestCommonAncestor(root.left, p, q);
        }
        
        // If p and q are on different sides of root (or one of them is the root)
        // then the root is their LCA
        return root;
    }
}
```

### 2. Iterative Approach
* Time complexity: O(h) where h is the height of the tree (worst case O(n) for skewed tree)
* Space complexity: O(1) as we only use a constant amount of extra space

#### Python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # Get values for convenience
        p_val = p.val
        q_val = q.val
        
        # Start from root
        current = root
        
        # Traverse the tree
        while current:
            # If both p and q are greater than current, go right
            if p_val > current.val and q_val > current.val:
                current = current.right
            # If both p and q are less than current, go left
            elif p_val < current.val and q_val < current.val:
                current = current.left
            # We have found the split point, this is the LCA
            else:
                return current
```

#### JavaScript
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function(root, p, q) {
    // Get values for convenience
    const pVal = p.val;
    const qVal = q.val;
    
    // Start from root
    let current = root;
    
    // Traverse the tree
    while (current) {
        // If both p and q are greater than current, go right
        if (pVal > current.val && qVal > current.val) {
            current = current.right;
        }
        // If both p and q are less than current, go left
        else if (pVal < current.val && qVal < current.val) {
            current = current.left;
        }
        // We have found the split point, this is the LCA
        else {
            return current;
        }
    }
    
    // This should never happen if p and q are in the tree
    return null;
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
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Get values for convenience
        int pVal = p.val;
        int qVal = q.val;
        
        // Start from root
        TreeNode current = root;
        
        // Traverse the tree
        while (current != null) {
            // If both p and q are greater than current, go right
            if (pVal > current.val && qVal > current.val) {
                current = current.right;
            }
            // If both p and q are less than current, go left
            else if (pVal < current.val && qVal < current.val) {
                current = current.left;
            }
            // We have found the split point, this is the LCA
            else {
                return current;
            }
        }
        
        // This should never happen if p and q are in the tree
        return null;
    }
}
```

### 3. Path Comparison Approach
* Time complexity: O(h) where h is the height of the tree (worst case O(n) for skewed tree)
* Space complexity: O(h) for storing the paths (worst case O(n) for skewed tree)

#### Python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # Find path from root to p
        path_p = []
        self.find_path(root, p, path_p)
        
        # Find path from root to q
        path_q = []
        self.find_path(root, q, path_q)
        
        # Find the last common node in the two paths
        lca = None
        for i in range(min(len(path_p), len(path_q))):
            if path_p[i] == path_q[i]:
                lca = path_p[i]
            else:
                break
                
        return lca
    
    def find_path(self, root, target, path):
        # Base case: empty tree
        if not root:
            return False
            
        # Add current node to path
        path.append(root)
        
        # If target is found, return True
        if root == target:
            return True
            
        # Determine which way to go based on BST property
        if target.val < root.val:
            found = self.find_path(root.left, target, path)
        else:
            found = self.find_path(root.right, target, path)
            
        # If target not found in this subtree, remove current node from path
        if not found:
            path.pop()
            
        return found
```

#### JavaScript
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function(root, p, q) {
    // Find path from root to p
    const pathP = [];
    findPath(root, p, pathP);
    
    // Find path from root to q
    const pathQ = [];
    findPath(root, q, pathQ);
    
    // Find the last common node in the two paths
    let lca = null;
    for (let i = 0; i < Math.min(pathP.length, pathQ.length); i++) {
        if (pathP[i] === pathQ[i]) {
            lca = pathP[i];
        } else {
            break;
        }
    }
    
    return lca;
};

function findPath(root, target, path) {
    // Base case: empty tree
    if (root === null) {
        return false;
    }
    
    // Add current node to path
    path.push(root);
    
    // If target is found, return true
    if (root === target) {
        return true;
    }
    
    // Determine which way to go based on BST property
    let found;
    if (target.val < root.val) {
        found = findPath(root.left, target, path);
    } else {
        found = findPath(root.right, target, path);
    }
    
    // If target not found in this subtree, remove current node from path
    if (!found) {
        path.pop();
    }
    
    return found;
}
```

#### Java
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Find path from root to p
        List<TreeNode> pathP = new ArrayList<>();
        findPath(root, p, pathP);
        
        // Find path from root to q
        List<TreeNode> pathQ = new ArrayList<>();
        findPath(root, q, pathQ);
        
        // Find the last common node in the two paths
        TreeNode lca = null;
        for (int i = 0; i < Math.min(pathP.size(), pathQ.size()); i++) {
            if (pathP.get(i) == pathQ.get(i)) {
                lca = pathP.get(i);
            } else {
                break;
            }
        }
        
        return lca;
    }
    
    private boolean findPath(TreeNode root, TreeNode target, List<TreeNode> path) {
        // Base case: empty tree
        if (root == null) {
            return false;
        }
        
        // Add current node to path
        path.add(root);
        
        // If target is found, return true
        if (root == target) {
            return true;
        }
        
        // Determine which way to go based on BST property
        boolean found;
        if (target.val < root.val) {
            found = findPath(root.left, target, path);
        } else {
            found = findPath(root.right, target, path);
        }
        
        // If target not found in this subtree, remove current node from path
        if (!found) {
            path.remove(path.size() - 1);
        }
        
        return found;
    }
}
```