# Subtree of Another Tree

## Problem Description
Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values as `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

## Approaches

### 1. Recursive Approach (Check Every Node)
* Time complexity: O(m * n) where m is the number of nodes in root tree and n is the number of nodes in subRoot tree
* Space complexity: O(h) where h is the height of the root tree (worst case O(m) for skewed tree)

#### Python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: TreeNode, subRoot: TreeNode) -> bool:
        # Base case: if root is None, subRoot cannot be a subtree
        if not root:
            return False
            
        # Check if the tree rooted at root is identical to subRoot
        if self.isSameTree(root, subRoot):
            return True
            
        # If not, check if subRoot is a subtree of the left or right subtree
        return (self.isSubtree(root.left, subRoot) or 
                self.isSubtree(root.right, subRoot))
    
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        # Base case: if both nodes are None, they are the same
        if not p and not q:
            return True
            
        # If one is None but the other is not, they are different
        if not p or not q:
            return False
            
        # If the values are different, they are different trees
        if p.val != q.val:
            return False
            
        # Recursively check if the left and right subtrees are identical
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
 * @param {TreeNode} root
 * @param {TreeNode} subRoot
 * @return {boolean}
 */
var isSubtree = function(root, subRoot) {
    // Base case: if root is null, subRoot cannot be a subtree
    if (root === null) {
        return false;
    }
    
    // Check if the tree rooted at root is identical to subRoot
    if (isSameTree(root, subRoot)) {
        return true;
    }
    
    // If not, check if subRoot is a subtree of the left or right subtree
    return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
};

// Helper function to check if two trees are identical
function isSameTree(p, q) {
    // Base case: if both nodes are null, they are the same
    if (p === null && q === null) {
        return true;
    }
    
    // If one is null but the other is not, they are different
    if (p === null || q === null) {
        return false;
    }
    
    // If the values are different, they are different trees
    if (p.val !== q.val) {
        return false;
    }
    
    // Recursively check if the left and right subtrees are identical
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
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
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        // Base case: if root is null, subRoot cannot be a subtree
        if (root == null) {
            return false;
        }
        
        // Check if the tree rooted at root is identical to subRoot
        if (isSameTree(root, subRoot)) {
            return true;
        }
        
        // If not, check if subRoot is a subtree of the left or right subtree
        return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
    }
    
    private boolean isSameTree(TreeNode p, TreeNode q) {
        // Base case: if both nodes are null, they are the same
        if (p == null && q == null) {
            return true;
        }
        
        // If one is null but the other is not, they are different
        if (p == null || q == null) {
            return false;
        }
        
        // If the values are different, they are different trees
        if (p.val != q.val) {
            return false;
        }
        
        // Recursively check if the left and right subtrees are identical
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```

### 2. String Serialization Approach
* Time complexity: O(m + n) where m is the number of nodes in root and n is the number of nodes in subRoot
* Space complexity: O(m + n) for the serialized strings

#### Python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: TreeNode, subRoot: TreeNode) -> bool:
        # Serialize both trees to strings with special markers
        # to avoid false positives
        serialized_root = self.serialize(root)
        serialized_subRoot = self.serialize(subRoot)
        
        # Check if serialized_subRoot is a substring of serialized_root
        return serialized_subRoot in serialized_root
    
    def serialize(self, root: TreeNode) -> str:
        # Perform a pre-order traversal with special markers for null nodes
        if not root:
            return "#"  # use # to mark null nodes
            
        # We use $ as a delimiter to ensure proper node boundaries
        # For example, 1,2 vs 12 will be $1$2$ vs $12$
        return "$" + str(root.val) + self.serialize(root.left) + self.serialize(root.right)
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
 * @param {TreeNode} subRoot
 * @return {boolean}
 */
var isSubtree = function(root, subRoot) {
    // Serialize both trees to strings with special markers
    // to avoid false positives
    const serializedRoot = serialize(root);
    const serializedSubRoot = serialize(subRoot);
    
    // Check if serializedSubRoot is a substring of serializedRoot
    return serializedRoot.includes(serializedSubRoot);
};

// Helper function to serialize a tree
function serialize(root) {
    // Perform a pre-order traversal with special markers for null nodes
    if (root === null) {
        return "#";  // use # to mark null nodes
    }
    
    // We use $ as a delimiter to ensure proper node boundaries
    // For example, 1,2 vs 12 will be $1$2$ vs $12$
    return "$" + root.val + serialize(root.left) + serialize(root.right);
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
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        // Serialize both trees to strings with special markers
        // to avoid false positives
        String serializedRoot = serialize(root);
        String serializedSubRoot = serialize(subRoot);
        
        // Check if serializedSubRoot is a substring of serializedRoot
        return serializedRoot.contains(serializedSubRoot);
    }
    
    private String serialize(TreeNode root) {
        // Perform a pre-order traversal with special markers for null nodes
        if (root == null) {
            return "#";  // use # to mark null nodes
        }
        
        // We use $ as a delimiter to ensure proper node boundaries
        // For example, 1,2 vs 12 will be $1$2$ vs $12$
        return "$" + root.val + serialize(root.left) + serialize(root.right);
    }
}
```

### 3. Iterative Approach
* Time complexity: O(m * n) where m is the number of nodes in root and n is the number of nodes in subRoot
* Space complexity: O(h1 + h2) where h1 and h2 are the heights of the trees (worst case O(m + n) for skewed trees)

#### Python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: TreeNode, subRoot: TreeNode) -> bool:
        # Handle edge case: if subRoot is None, it's a subtree
        if not subRoot:
            return True
            
        # Handle edge case: if root is None but subRoot is not, it's not a subtree
        if not root:
            return False
            
        # Use stack for DFS traversal
        stack = [root]
        
        while stack:
            node = stack.pop()
            
            # If the current node value matches the subRoot value,
            # check if the entire tree starting from this node matches
            if node.val == subRoot.val and self.isSameTree(node, subRoot):
                return True
                
            # Add non-null children to the stack
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
                
        return False
    
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        # Use stack for iterative comparison
        stack = [(p, q)]
        
        while stack:
            node1, node2 = stack.pop()
            
            # If both nodes are None, continue
            if not node1 and not node2:
                continue
                
            # If one is None or values don't match, trees are different
            if not node1 or not node2 or node1.val != node2.val:
                return False
                
            # Add children pairs to stack
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
 * @param {TreeNode} root
 * @param {TreeNode} subRoot
 * @return {boolean}
 */
var isSubtree = function(root, subRoot) {
    // Handle edge case: if subRoot is null, it's a subtree
    if (subRoot === null) {
        return true;
    }
    
    // Handle edge case: if root is null but subRoot is not, it's not a subtree
    if (root === null) {
        return false;
    }
    
    // Use stack for DFS traversal
    const stack = [root];
    
    while (stack.length > 0) {
        const node = stack.pop();
        
        // If the current node value matches the subRoot value,
        // check if the entire tree starting from this node matches
        if (node.val === subRoot.val && isSameTree(node, subRoot)) {
            return true;
        }
        
        // Add non-null children to the stack
        if (node.right !== null) {
            stack.push(node.right);
        }
        if (node.left !== null) {
            stack.push(node.left);
        }
    }
    
    return false;
};

function isSameTree(p, q) {
    // Use stack for iterative comparison
    const stack = [[p, q]];
    
    while (stack.length > 0) {
        const [node1, node2] = stack.pop();
        
        // If both nodes are null, continue
        if (node1 === null && node2 === null) {
            continue;
        }
        
        // If one is null or values don't match, trees are different
        if (node1 === null || node2 === null || node1.val !== node2.val) {
            return false;
        }
        
        // Add children pairs to stack
        stack.push([node1.right, node2.right]);
        stack.push([node1.left, node2.left]);
    }
    
    return true;
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
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        // Handle edge case: if subRoot is null, it's a subtree
        if (subRoot == null) {
            return true;
        }
        
        // Handle edge case: if root is null but subRoot is not, it's not a subtree
        if (root == null) {
            return false;
        }
        
        // Use stack for DFS traversal
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            
            // If the current node value matches the subRoot value,
            // check if the entire tree starting from this node matches
            if (node.val == subRoot.val && isSameTree(node, subRoot)) {
                return true;
            }
            
            // Add non-null children to the stack
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }
        
        return false;
    }
    
    private boolean isSameTree(TreeNode p, TreeNode q) {
        // Use stack for iterative comparison
        Stack<TreeNode[]> stack = new Stack<>();
        stack.push(new TreeNode[]{p, q});
        
        while (!stack.isEmpty()) {
            TreeNode[] pair = stack.pop();
            TreeNode node1 = pair[0];
            TreeNode node2 = pair[1];
            
            // If both nodes are null, continue
            if (node1 == null && node2 == null) {
                continue;
            }
            
            // If one is null or values don't match, trees are different
            if (node1 == null || node2 == null || node1.val != node2.val) {
                return false;
            }
            
            // Add children pairs to stack
            stack.push(new TreeNode[]{node1.right, node2.right});
            stack.push(new TreeNode[]{node1.left, node2.left});
        }
        
        return true;
    }
}
```