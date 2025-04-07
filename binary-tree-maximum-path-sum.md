# Binary Tree Maximum Path Sum

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

## Examples

### Example 1:
```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```

### Example 2:
```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```

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

### 2. Depth First Search (CodeStoryWithMik's Approach) ✅

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def __init__(self):
        self.maxSum = float('-inf')
        
    def solve(self, root: Optional[TreeNode]) -> int:
        if root is None:
            return 0
            
        l = self.solve(root.left)
        r = self.solve(root.right)
        
        sabhi_ache = l + r + root.val        # All nodes good (complete path through current node)
        koi_ek_acha = max(l, r) + root.val   # One branch good (path from current node to one direction)
        only_root_acha = root.val            # Only current node good (path is just this node)
        
        self.maxSum = max(self.maxSum, sabhi_ache, koi_ek_acha, only_root_acha)
        
        return max(koi_ek_acha, only_root_acha)
        
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        self.solve(root)
        return self.maxSum
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
    let maxSum = -Infinity;
    
    function solve(node) {
        if (!node) return 0;
        
        const l = solve(node.left);
        const r = solve(node.right);
        
        const sabhi_ache = l + r + node.val;        // All nodes good (complete path through current node)
        const koi_ek_acha = Math.max(l, r) + node.val;   // One branch good (path from node to one direction)
        const only_root_acha = node.val;            // Only current node good (path is just this node)
        
        maxSum = Math.max(maxSum, sabhi_ache, koi_ek_acha, only_root_acha);
        
        return Math.max(koi_ek_acha, only_root_acha);
    }
    
    solve(root);
    return maxSum;
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
    private int maxSum = Integer.MIN_VALUE;
    
    public int maxPathSum(TreeNode root) {
        solve(root);
        return maxSum;
    }
    
    private int solve(TreeNode root) {
        if (root == null) return 0;
        
        int l = solve(root.left);
        int r = solve(root.right);
        
        int sabhi_ache = l + r + root.val;        // All nodes good (complete path through current node)
        int koi_ek_acha = Math.max(l, r) + root.val;   // One branch good (path from node to one direction)
        int only_root_acha = root.val;            // Only current node good (path is just this node)
        
        maxSum = Math.max(maxSum, Math.max(sabhi_ache, Math.max(koi_ek_acha, only_root_acha)));
        
        return Math.max(koi_ek_acha, only_root_acha);
    }
}
```

## Intuition

The key insight for solving this problem is understanding that a valid path in a binary tree:
1. Can start and end at any node (doesn't need to include the root)
2. Cannot have any node appear more than once (no cycles)
3. Must follow the tree edges

For any given node, there are four possible path configurations:
1. `sabhi_ache`: Path includes the current node and both its left and right subtrees (complete path through current node)
2. `koi_ek_acha`: Path includes the current node and either its left OR right subtree (path from current node to one direction)
3. `only_root_acha`: Path is just the current node alone (path consists of only the current node)
4. No path through this node (when all values are negative)

CodeStoryWithMik's solution uses post-order traversal where we:
1. Recursively find the maximum path sum from each subtree
2. For each node, consider all possible path configurations:
   - Complete path through the node (`sabhi_ache`)
   - Path from node to one direction (`koi_ek_acha`)
   - Just the node itself (`only_root_acha`)
3. Update the global maximum by comparing with the current path options
4. Return the maximum usable path (either `koi_ek_acha` or `only_root_acha`) for use by parent nodes

This approach effectively handles negative values by considering the case where we might want to exclude certain subtrees entirely.
