# Construct Binary Tree from Preorder and Inorder Traversal

Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.

## Approaches

### 1. Depth First Search ✅

**Time complexity:** O(n²)  
**Space complexity:** O(n²)

#### Python
```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder:
            return None
            
        root = TreeNode(preorder[0])
        mid = inorder.index(preorder[0])
        root.left = self.buildTree(preorder[1 : mid + 1], inorder[:mid])
        root.right = self.buildTree(preorder[mid + 1 :], inorder[mid + 1 :])
        
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
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    if (preorder.length === 0 || inorder.length === 0) {
        return null;
    }
    
    const rootVal = preorder[0];
    const root = new TreeNode(rootVal);
    
    const mid = inorder.indexOf(rootVal);
    
    root.left = buildTree(
        preorder.slice(1, mid + 1),
        inorder.slice(0, mid)
    );
    
    root.right = buildTree(
        preorder.slice(mid + 1),
        inorder.slice(mid + 1)
    );
    
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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder.length == 0 || inorder.length == 0) {
            return null;
        }
        
        TreeNode root = new TreeNode(preorder[0]);
        int mid = 0;
        
        for (int i = 0; i < inorder.length; i++) {
            if (inorder[i] == preorder[0]) {
                mid = i;
                break;
            }
        }
        
        root.left = buildTree(
            Arrays.copyOfRange(preorder, 1, mid + 1),
            Arrays.copyOfRange(inorder, 0, mid)
        );
        
        root.right = buildTree(
            Arrays.copyOfRange(preorder, mid + 1, preorder.length),
            Arrays.copyOfRange(inorder, mid + 1, inorder.length)
        );
        
        return root;
    }
}
```

### 2. Hash Map

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        indices = {val: idx for idx, val in enumerate(inorder)}
        self.pre_idx = 0
        
        def dfs(l, r):
            if l > r:
                return None
                
            root_val = preorder[self.pre_idx]
            self.pre_idx += 1
            
            root = TreeNode(root_val)
            mid = indices[root_val]
            
            root.left = dfs(l, mid - 1)
            root.right = dfs(mid + 1, r)
            
            return root
            
        return dfs(0, len(inorder) - 1)
```

#### JavaScript
```javascript
/**
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    const indices = new Map();
    
    // Map each value in inorder array to its index
    for (let i = 0; i < inorder.length; i++) {
        indices.set(inorder[i], i);
    }
    
    let preIdx = 0;
    
    function dfs(left, right) {
        if (left > right) {
            return null;
        }
        
        const rootVal = preorder[preIdx++];
        const root = new TreeNode(rootVal);
        
        const mid = indices.get(rootVal);
        
        root.left = dfs(left, mid - 1);
        root.right = dfs(mid + 1, right);
        
        return root;
    }
    
    return dfs(0, inorder.length - 1);
};
```

#### Java
```java
class Solution {
    private int preIdx;
    private Map<Integer, Integer> inorderMap;
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        preIdx = 0;
        inorderMap = new HashMap<>();
        
        // Map each value in inorder array to its index
        for (int i = 0; i < inorder.length; i++) {
            inorderMap.put(inorder[i], i);
        }
        
        return dfs(preorder, 0, inorder.length - 1);
    }
    
    private TreeNode dfs(int[] preorder, int left, int right) {
        if (left > right) {
            return null;
        }
        
        int rootVal = preorder[preIdx++];
        TreeNode root = new TreeNode(rootVal);
        
        int mid = inorderMap.get(rootVal);
        
        root.left = dfs(preorder, left, mid - 1);
        root.right = dfs(preorder, mid + 1, right);
        
        return root;
    }
}
```

### 3. Depth First Search (Optimal)

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        preIdx = inIdx = 0
        
        def dfs(limit):
            nonlocal preIdx, inIdx
            
            if preIdx >= len(preorder):
                return None
                
            if inorder[inIdx] == limit:
                inIdx += 1
                return None
                
            root = TreeNode(preorder[preIdx])
            preIdx += 1
            
            root.left = dfs(root.val)
            root.right = dfs(limit)
            
            return root
            
        return dfs(float('inf'))
```

#### JavaScript
```javascript
/**
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    let preIdx = 0;
    let inIdx = 0;
    
    function dfs(limit) {
        if (preIdx >= preorder.length) {
            return null;
        }
        
        if (inorder[inIdx] === limit) {
            inIdx++;
            return null;
        }
        
        const root = new TreeNode(preorder[preIdx++]);
        
        root.left = dfs(root.val);
        root.right = dfs(limit);
        
        return root;
    }
    
    return dfs(Infinity);
};
```

#### Java
```java
class Solution {
    private int preIdx, inIdx;
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        preIdx = 0;
        inIdx = 0;
        
        return dfs(preorder, inorder, Integer.MAX_VALUE);
    }
    
    private TreeNode dfs(int[] preorder, int[] inorder, int limit) {
        if (preIdx >= preorder.length) {
            return null;
        }
        
        if (inorder[inIdx] == limit) {
            inIdx++;
            return null;
        }
        
        TreeNode root = new TreeNode(preorder[preIdx++]);
        
        root.left = dfs(preorder, inorder, root.val);
        root.right = dfs(preorder, inorder, limit);
        
        return root;
    }
}
```
