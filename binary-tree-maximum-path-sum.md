# Binary Tree Maximum Path Sum

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

## Approaches

### 1. Depth First Search

**Time complexity:** O(n²)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        res = -float('inf')
        
        def dfs(root):
            nonlocal res
            if not root:
                return
                
            left = self.getMax(root.left)
            right = self.getMax(root.right)
            res = max(res, root.val + left + right)
            
            dfs(root.left)
            dfs(root.right)
            
        dfs(root)
        return res
        
    def getMax(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
            
        left = self.getMax(root.left)
        right = self.getMax(root.right)
        path = root.val + max(left, right)
        
        return max(0, path)
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
var maxPathSum = function(root) {
    let result = -Infinity;
    
    function dfs(node) {
        if (!node) return;
        
        const left = getMax(node.left);
        const right = getMax(node.right);
        
        result = Math.max(result, node.val + left + right);
        
        dfs(node.left);
        dfs(node.right);
    }
    
    function getMax(node) {
        if (!node) return 0;
        
        const left = getMax(node.left);
        const right = getMax(node.right);
        const path = node.val + Math.max(left, right);
        
        return Math.max(0, path);
    }
    
    dfs(root);
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
    private int result = Integer.MIN_VALUE;
    
    public int maxPathSum(TreeNode root) {
        dfs(root);
        return result;
    }
    
    private void dfs(TreeNode root) {
        if (root == null) return;
        
        int left = getMax(root.left);
        int right = getMax(root.right);
        
        result = Math.max(result, root.val + left + right);
        
        dfs(root.left);
        dfs(root.right);
    }
    
    private int getMax(TreeNode root) {
        if (root == null) return 0;
        
        int left = getMax(root.left);
        int right = getMax(root.right);
        int path = root.val + Math.max(left, right);
        
        return Math.max(0, path);
    }
}
```

### 2. Depth First Search (Optimal)

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        res = [root.val]
        
        def dfs(root):
            if not root:
                return 0
                
            leftMax = dfs(root.left)
            rightMax = dfs(root.right)
            
            leftMax = max(leftMax, 0)
            rightMax = max(rightMax, 0)
            
            res[0] = max(res[0], root.val + leftMax + rightMax)
            
            return root.val + max(leftMax, rightMax)
            
        dfs(root)
        return res[0]
```

#### JavaScript
```javascript
/**
 * @param {TreeNode} root
 * @return {number}
 */
var maxPathSum = function(root) {
    const result = [root.val];
    
    function dfs(node) {
        if (!node) return 0;
        
        let leftMax = dfs(node.left);
        let rightMax = dfs(node.right);
        
        leftMax = Math.max(leftMax, 0);
        rightMax = Math.max(rightMax, 0);
        
        result[0] = Math.max(result[0], node.val + leftMax + rightMax);
        
        return node.val + Math.max(leftMax, rightMax);
    }
    
    dfs(root);
    return result[0];
};
```

#### Java
```java
class Solution {
    public int maxPathSum(TreeNode root) {
        int[] result = new int[]{root.val};
        
        dfs(root, result);
        return result[0];
    }
    
    private int dfs(TreeNode node, int[] result) {
        if (node == null) return 0;
        
        int leftMax = Math.max(dfs(node.left, result), 0);
        int rightMax = Math.max(dfs(node.right, result), 0);
        
        result[0] = Math.max(result[0], node.val + leftMax + rightMax);
        
        return node.val + Math.max(leftMax, rightMax);
    }
}
```