# Kth Smallest Element in a BST

Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

## Approaches

### 1. Brute Force

**Time complexity:** O(n log n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        arr = []
        
        def dfs(node):
            if not node:
                return
                
            arr.append(node.val)
            dfs(node.left)
            dfs(node.right)
            
        dfs(root)
        arr.sort()
        return arr[k - 1]
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
 * @param {number} k
 * @return {number}
 */
var kthSmallest = function(root, k) {
    const arr = [];
    
    function dfs(node) {
        if (!node) return;
        
        arr.push(node.val);
        dfs(node.left);
        dfs(node.right);
    }
    
    dfs(root);
    arr.sort((a, b) => a - b);
    return arr[k - 1];
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
    public int kthSmallest(TreeNode root, int k) {
        List<Integer> arr = new ArrayList<>();
        
        dfs(root, arr);
        Collections.sort(arr);
        return arr.get(k - 1);
    }
    
    private void dfs(TreeNode node, List<Integer> arr) {
        if (node == null) return;
        
        arr.add(node.val);
        dfs(node.left, arr);
        dfs(node.right, arr);
    }
}
```

### 2. Inorder Traversal

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        arr = []
        
        def dfs(node):
            if not node:
                return
                
            dfs(node.left)
            arr.append(node.val)
            dfs(node.right)
            
        dfs(root)
        return arr[k - 1]
```

#### JavaScript
```javascript
/**
 * @param {TreeNode} root
 * @param {number} k
 * @return {number}
 */
var kthSmallest = function(root, k) {
    const arr = [];
    
    function inorder(node) {
        if (!node) return;
        
        inorder(node.left);
        arr.push(node.val);
        inorder(node.right);
    }
    
    inorder(root);
    return arr[k - 1];
};
```

#### Java
```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        List<Integer> arr = new ArrayList<>();
        
        inorder(root, arr);
        return arr.get(k - 1);
    }
    
    private void inorder(TreeNode node, List<Integer> arr) {
        if (node == null) return;
        
        inorder(node.left, arr);
        arr.add(node.val);
        inorder(node.right, arr);
    }
}
```

### 3. Recursive DFS (Optimal)

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        cnt = k
        res = root.val
        
        def dfs(node):
            nonlocal cnt, res
            if not node:
                return
                
            dfs(node.left)
            cnt -= 1
            if cnt == 0:
                res = node.val
                return
                
            dfs(node.right)
            
        dfs(root)
        return res
```

#### JavaScript
```javascript
/**
 * @param {TreeNode} root
 * @param {number} k
 * @return {number}
 */
var kthSmallest = function(root, k) {
    let count = k;
    let result = root.val;
    
    function dfs(node) {
        if (!node) return;
        
        dfs(node.left);
        count--;
        
        if (count === 0) {
            result = node.val;
            return;
        }
        
        dfs(node.right);
    }
    
    dfs(root);
    return result;
};
```

#### Java
```java
class Solution {
    private int count;
    private int result;
    
    public int kthSmallest(TreeNode root, int k) {
        count = k;
        result = root.val;
        
        dfs(root);
        return result;
    }
    
    private void dfs(TreeNode node) {
        if (node == null) return;
        
        dfs(node.left);
        count--;
        
        if (count == 0) {
            result = node.val;
            return;
        }
        
        dfs(node.right);
    }
}
```

### 4. Iterative DFS (Optimal)

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        stack = []
        curr = root
        
        while stack or curr:
            while curr:
                stack.append(curr)
                curr = curr.left
                
            curr = stack.pop()
            k -= 1
            if k == 0:
                return curr.val
                
            curr = curr.right
```

#### JavaScript
```javascript
/**
 * @param {TreeNode} root
 * @param {number} k
 * @return {number}
 */
var kthSmallest = function(root, k) {
    const stack = [];
    let curr = root;
    
    while (stack.length > 0 || curr) {
        while (curr) {
            stack.push(curr);
            curr = curr.left;
        }
        
        curr = stack.pop();
        k--;
        
        if (k === 0) {
            return curr.val;
        }
        
        curr = curr.right;
    }
};
```

#### Java
```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
        
        while (!stack.isEmpty() || curr != null) {
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            
            curr = stack.pop();
            k--;
            
            if (k == 0) {
                return curr.val;
            }
            
            curr = curr.right;
        }
        
        return -1; // Should never reach this
    }
}
```