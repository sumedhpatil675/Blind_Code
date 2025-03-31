# Validate Binary Search Tree

Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:
- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

## Approaches

### 1. Brute Force

**Time complexity:** O(n²)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    left_check = staticmethod(lambda val, limit: val < limit)
    right_check = staticmethod(lambda val, limit: val > limit)

    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
            
        if (not self.isValid(root.left, root.val, self.left_check) or
            not self.isValid(root.right, root.val, self.right_check)):
            return False
            
        return self.isValidBST(root.left) and self.isValidBST(root.right)
        
    def isValid(self, root: Optional[TreeNode], limit: int, check) -> bool:
        if not root:
            return True
            
        if not check(root.val, limit):
            return False
            
        return (self.isValid(root.left, limit, check) and
                self.isValid(root.right, limit, check))
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
 * @return {boolean}
 */
var isValidBST = function(root) {
    const leftCheck = (val, limit) => val < limit;
    const rightCheck = (val, limit) => val > limit;
    
    function isValid(node, limit, check) {
        if (!node) return true;
        
        if (!check(node.val, limit)) return false;
        
        return isValid(node.left, limit, check) && 
               isValid(node.right, limit, check);
    }
    
    function checkBST(node) {
        if (!node) return true;
        
        if (!isValid(node.left, node.val, leftCheck) ||
            !isValid(node.right, node.val, rightCheck)) {
            return false;
        }
        
        return checkBST(node.left) && checkBST(node.right);
    }
    
    return checkBST(root);
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
    public boolean isValidBST(TreeNode root) {
        return checkBST(root);
    }
    
    private boolean checkBST(TreeNode root) {
        if (root == null) return true;
        
        if (!isValid(root.left, root.val, true) || 
            !isValid(root.right, root.val, false)) {
            return false;
        }
        
        return checkBST(root.left) && checkBST(root.right);
    }
    
    private boolean isValid(TreeNode root, int limit, boolean isLeft) {
        if (root == null) return true;
        
        if (isLeft) {
            if (root.val >= limit) return false;
        } else {
            if (root.val <= limit) return false;
        }
        
        return isValid(root.left, limit, isLeft) && 
               isValid(root.right, limit, isLeft);
    }
}
```

### 2. Depth First Search

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def valid(node, left, right):
            if not node:
                return True
                
            if not (left < node.val < right):
                return False
                
            return valid(node.left, left, node.val) and valid(
                node.right, node.val, right
            )
            
        return valid(root, float("-inf"), float("inf"))
```

#### JavaScript
```javascript
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isValidBST = function(root) {
    function valid(node, left, right) {
        if (!node) return true;
        
        if (!(node.val > left && node.val < right)) return false;
        
        return valid(node.left, left, node.val) && valid(node.right, node.val, right);
    }
    
    return valid(root, -Infinity, Infinity);
};
```

#### Java
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return valid(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    private boolean valid(TreeNode node, long left, long right) {
        if (node == null) return true;
        
        if (!(node.val > left && node.val < right)) {
            return false;
        }
        
        return valid(node.left, left, node.val) && 
               valid(node.right, node.val, right);
    }
}
```

### 3. Breadth First Search

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
            
        q = deque([(root, float("-inf"), float("inf"))])
        
        while q:
            node, left, right = q.popleft()
            
            if not (left < node.val < right):
                return False
                
            if node.left:
                q.append((node.left, left, node.val))
                
            if node.right:
                q.append((node.right, node.val, right))
                
        return True
```

#### JavaScript
```javascript
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isValidBST = function(root) {
    if (!root) return true;
    
    const queue = [[root, -Infinity, Infinity]];
    
    while (queue.length > 0) {
        const [node, left, right] = queue.shift();
        
        if (!(node.val > left && node.val < right)) {
            return false;
        }
        
        if (node.left) {
            queue.push([node.left, left, node.val]);
        }
        
        if (node.right) {
            queue.push([node.right, node.val, right]);
        }
    }
    
    return true;
};
```

#### Java
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        
        Queue<Object[]> queue = new LinkedList<>();
        queue.offer(new Object[]{root, Long.MIN_VALUE, Long.MAX_VALUE});
        
        while (!queue.isEmpty()) {
            Object[] curr = queue.poll();
            TreeNode node = (TreeNode) curr[0];
            long left = (long) curr[1];
            long right = (long) curr[2];
            
            if (!(node.val > left && node.val < right)) {
                return false;
            }
            
            if (node.left != null) {
                queue.offer(new Object[]{node.left, left, (long) node.val});
            }
            
            if (node.right != null) {
                queue.offer(new Object[]{node.right, (long) node.val, right});
            }
        }
        
        return true;
    }
}
```