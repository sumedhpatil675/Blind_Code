# Serialize and Deserialize Binary Tree

Implement an algorithm to serialize and deserialize a binary tree. Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

## Approaches

### 1. Depth First Search ✅✅✅

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Codec:
    # Encodes a tree to a single string.
    def serialize(self, root: Optional[TreeNode]) -> str:
        res = []
        
        def dfs(node):
            if not node:
                res.append("N")
                return
                
            res.append(str(node.val))
            dfs(node.left)
            dfs(node.right)
            
        dfs(root)
        return ",".join(res)
        
    # Decodes your encoded data to tree.
    def deserialize(self, data: str) -> Optional[TreeNode]:
        vals = data.split(",")
        self.i = 0
        
        def dfs():
            if vals[self.i] == "N":
                self.i += 1
                return None
                
            node = TreeNode(int(vals[self.i]))
            self.i += 1
            
            node.left = dfs()
            node.right = dfs()
            
            return node
            
        return dfs()
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
 * Encodes a tree to a single string.
 *
 * @param {TreeNode} root
 * @return {string}
 */
var serialize = function(root) {
    const result = [];
    
    function dfs(node) {
        if (!node) {
            result.push("N");
            return;
        }
        
        result.push(node.val.toString());
        dfs(node.left);
        dfs(node.right);
    }
    
    dfs(root);
    return result.join(",");
};

/**
 * Decodes your encoded data to tree.
 *
 * @param {string} data
 * @return {TreeNode}
 */
var deserialize = function(data) {
    const vals = data.split(",");
    let i = 0;
    
    function dfs() {
        if (vals[i] === "N") {
            i++;
            return null;
        }
        
        const node = new TreeNode(parseInt(vals[i]));
        i++;
        
        node.left = dfs();
        node.right = dfs();
        
        return node;
    }
    
    return dfs();
};

/**
 * Your functions will be called as such:
 * deserialize(serialize(root));
 */
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
public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serializeDFS(root, sb);
        return sb.toString();
    }
    
    private void serializeDFS(TreeNode node, StringBuilder sb) {
        if (node == null) {
            sb.append("N,");
            return;
        }
        
        sb.append(node.val).append(",");
        serializeDFS(node.left, sb);
        serializeDFS(node.right, sb);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        Queue<String> queue = new LinkedList<>(Arrays.asList(data.split(",")));
        return deserializeDFS(queue);
    }
    
    private TreeNode deserializeDFS(Queue<String> queue) {
        String val = queue.poll();
        
        if ("N".equals(val)) {
            return null;
        }
        
        TreeNode node = new TreeNode(Integer.parseInt(val));
        node.left = deserializeDFS(queue);
        node.right = deserializeDFS(queue);
        
        return node;
    }
}
```

### 2. Breadth First Search

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python
```python
class Codec:
    # Encodes a tree to a single string.
    def serialize(self, root: Optional[TreeNode]) -> str:
        if not root:
            return "N"
            
        res = []
        queue = deque([root])
        
        while queue:
            node = queue.popleft()
            
            if not node:
                res.append("N")
            else:
                res.append(str(node.val))
                queue.append(node.left)
                queue.append(node.right)
                
        return ",".join(res)
        
    # Decodes your encoded data to tree.
    def deserialize(self, data: str) -> Optional[TreeNode]:
        vals = data.split(",")
        
        if vals[0] == "N":
            return None
            
        root = TreeNode(int(vals[0]))
        queue = deque([root])
        index = 1
        
        while queue:
            node = queue.popleft()
            
            if vals[index] != "N":
                node.left = TreeNode(int(vals[index]))
                queue.append(node.left)
            index += 1
            
            if vals[index] != "N":
                node.right = TreeNode(int(vals[index]))
                queue.append(node.right)
            index += 1
            
        return root
```

#### JavaScript
```javascript
/**
 * Encodes a tree to a single string.
 *
 * @param {TreeNode} root
 * @return {string}
 */
var serialize = function(root) {
    if (!root) return "N";
    
    const result = [];
    const queue = [root];
    
    while (queue.length > 0) {
        const node = queue.shift();
        
        if (!node) {
            result.push("N");
        } else {
            result.push(node.val.toString());
            queue.push(node.left);
            queue.push(node.right);
        }
    }
    
    return result.join(",");
};

/**
 * Decodes your encoded data to tree.
 *
 * @param {string} data
 * @return {TreeNode}
 */
var deserialize = function(data) {
    const vals = data.split(",");
    
    if (vals[0] === "N") return null;
    
    const root = new TreeNode(parseInt(vals[0]));
    const queue = [root];
    let index = 1;
    
    while (queue.length > 0) {
        const node = queue.shift();
        
        if (vals[index] !== "N") {
            node.left = new TreeNode(parseInt(vals[index]));
            queue.push(node.left);
        }
        index++;
        
        if (vals[index] !== "N") {
            node.right = new TreeNode(parseInt(vals[index]));
            queue.push(node.right);
        }
        index++;
    }
    
    return root;
};
```

#### Java
```java
public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) return "N";
        
        StringBuilder sb = new StringBuilder();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            
            if (node == null) {
                sb.append("N,");
            } else {
                sb.append(node.val).append(",");
                queue.offer(node.left);
                queue.offer(node.right);
            }
        }
        
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] vals = data.split(",");
        
        if ("N".equals(vals[0])) return null;
        
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        int index = 1;
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            
            if (!vals[index].equals("N")) {
                node.left = new TreeNode(Integer.parseInt(vals[index]));
                queue.offer(node.left);
            }
            index++;
            
            if (!vals[index].equals("N")) {
                node.right = new TreeNode(Integer.parseInt(vals[index]));
                queue.offer(node.right);
            }
            index++;
        }
        
        return root;
    }
}
```
