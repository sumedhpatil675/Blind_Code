# Remove Nth Node From End of List

## Problem Description
Given the head of a linked list, remove the nth node from the end of the list and return its head.

## Approaches

### 1. Two-Pass Approach (Count Length First)
* Time complexity: O(n) where n is the length of the linked list
* Space complexity: O(1)

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # First pass: count the length of the linked list
        length = 0
        curr = head
        while curr:
            length += 1
            curr = curr.next
            
        # Handle edge case: if we need to remove the head node
        if length == n:
            return head.next
            
        # Second pass: find the node before the one to be removed
        curr = head
        for i in range(length - n - 1):
            curr = curr.next
            
        # Remove the nth node from the end
        curr.next = curr.next.next
        
        return head
```

#### JavaScript
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    // First pass: count the length of the linked list
    let length = 0;
    let curr = head;
    while (curr) {
        length++;
        curr = curr.next;
    }
    
    // Handle edge case: if we need to remove the head node
    if (length === n) {
        return head.next;
    }
    
    // Second pass: find the node before the one to be removed
    curr = head;
    for (let i = 0; i < length - n - 1; i++) {
        curr = curr.next;
    }
    
    // Remove the nth node from the end
    curr.next = curr.next.next;
    
    return head;
};
```

#### Java
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // First pass: count the length of the linked list
        int length = 0;
        ListNode curr = head;
        while (curr != null) {
            length++;
            curr = curr.next;
        }
        
        // Handle edge case: if we need to remove the head node
        if (length == n) {
            return head.next;
        }
        
        // Second pass: find the node before the one to be removed
        curr = head;
        for (int i = 0; i < length - n - 1; i++) {
            curr = curr.next;
        }
        
        // Remove the nth node from the end
        curr.next = curr.next.next;
        
        return head;
    }
}
```

### 2. One-Pass Approach (Two Pointers)
* Time complexity: O(n) where n is the length of the linked list
* Space complexity: O(1)

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # Create a dummy node to handle the edge case of removing the head
        dummy = ListNode(0, head)
        
        # Initialize two pointers
        left = dummy
        right = head
        
        # Move right pointer n steps ahead
        for i in range(n):
            right = right.next
            
        # Move both pointers until right reaches the end
        while right:
            left = left.next
            right = right.next
            
        # Remove the nth node from the end
        left.next = left.next.next
        
        return dummy.next
```

#### JavaScript
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    // Create a dummy node to handle the edge case of removing the head
    const dummy = new ListNode(0, head);
    
    // Initialize two pointers
    let left = dummy;
    let right = head;
    
    // Move right pointer n steps ahead
    for (let i = 0; i < n; i++) {
        right = right.next;
    }
    
    // Move both pointers until right reaches the end
    while (right) {
        left = left.next;
        right = right.next;
    }
    
    // Remove the nth node from the end
    left.next = left.next.next;
    
    return dummy.next;
};
```

#### Java
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // Create a dummy node to handle the edge case of removing the head
        ListNode dummy = new ListNode(0, head);
        
        // Initialize two pointers
        ListNode left = dummy;
        ListNode right = head;
        
        // Move right pointer n steps ahead
        for (int i = 0; i < n; i++) {
            right = right.next;
        }
        
        // Move both pointers until right reaches the end
        while (right != null) {
            left = left.next;
            right = right.next;
        }
        
        // Remove the nth node from the end
        left.next = left.next.next;
        
        return dummy.next;
    }
}
```

### 3. Recursive Approach
* Time complexity: O(n) where n is the length of the linked list
* Space complexity: O(n) due to the recursion stack

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # This is a helper function that will traverse the list and return the position from the end
        def traverse(node):
            if not node:
                return 0
                
            # Recurse to the end of the list
            position = traverse(node.next) + 1
            
            # On the way back, if this is the node to remove
            if position == n + 1:
                # Skip the next node (which is the nth from the end)
                node.next = node.next.next if node.next else None
                
            return position
            
        # Handle the edge case where the head is the nth node from the end
        dummy = ListNode(0, head)
        traverse(dummy)
        return dummy.next
```

#### JavaScript
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    // Create a dummy node to handle edge cases
    const dummy = new ListNode(0);
    dummy.next = head;
    
    // This function returns the position from the end
    function traverse(node) {
        if (!node) {
            return 0;
        }
        
        // Recurse to the end of the list
        const position = traverse(node.next) + 1;
        
        // On the way back, if this is the node before the one to remove
        if (position === n + 1) {
            // Skip the next node (which is the nth from the end)
            node.next = node.next ? node.next.next : null;
        }
        
        return position;
    }
    
    traverse(dummy);
    return dummy.next;
};
```

#### Java
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    private int count;
    
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // Create a dummy node to handle edge cases
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        count = 0;
        
        traverse(dummy, n);
        return dummy.next;
    }
    
    private void traverse(ListNode node, int n) {
        if (node == null) {
            return;
        }
        
        // Recurse to the end of the list
        traverse(node.next, n);
        
        // On the way back, count the nodes
        count++;
        
        // If this is the node before the one to remove
        if (count == n + 1) {
            // Skip the next node (which is the nth from the end)
            node.next = node.next != null ? node.next.next : null;
        }
    }
}
```