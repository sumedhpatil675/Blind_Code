# Linked List Cycle

## Problem Description
Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. Note that `pos` is not passed as a parameter.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

## Approaches

### 1. Hash Set Approach
* Time complexity: O(n) where n is the number of nodes in the linked list
* Space complexity: O(n) for storing all nodes in the hash set

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        # Use a hash set to keep track of visited nodes
        seen = set()
        
        # Traverse the linked list
        current = head
        while current:
            # If we've seen this node before, there's a cycle
            if current in seen:
                return True
                
            # Add the current node to the set
            seen.add(current)
            
            # Move to the next node
            current = current.next
            
        # If we reach the end of the list without finding a cycle
        return False
```

#### JavaScript
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    // Use a Set to keep track of visited nodes
    const seen = new Set();
    
    // Traverse the linked list
    let current = head;
    while (current) {
        // If we've seen this node before, there's a cycle
        if (seen.has(current)) {
            return true;
        }
        
        // Add the current node to the set
        seen.add(current);
        
        // Move to the next node
        current = current.next;
    }
    
    // If we reach the end of the list without finding a cycle
    return false;
};
```

#### Java
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        // Use a HashSet to keep track of visited nodes
        Set<ListNode> seen = new HashSet<>();
        
        // Traverse the linked list
        ListNode current = head;
        while (current != null) {
            // If we've seen this node before, there's a cycle
            if (seen.contains(current)) {
                return true;
            }
            
            // Add the current node to the set
            seen.add(current);
            
            // Move to the next node
            current = current.next;
        }
        
        // If we reach the end of the list without finding a cycle
        return false;
    }
}
```

### 2. Floyd's Cycle-Finding Algorithm (Slow and Fast Pointers) ✅✅✅

* Time complexity: O(n) where n is the number of nodes in the linked list
* Space complexity: O(1) since we only use two pointers

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        # Handle edge case of empty list
        if not head:
            return False
        
        # Initialize slow and fast pointers
        slow = head
        fast = head
        
        # Move slow one step and fast two steps at a time
        # If there's a cycle, fast will eventually catch up to slow
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            
            # If slow and fast meet, there's a cycle
            if slow == fast:
                return True
                
        # If fast reaches the end, there's no cycle
        return False
```

#### JavaScript
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    // Handle edge case of empty list
    if (!head) {
        return false;
    }
    
    // Initialize slow and fast pointers
    let slow = head;
    let fast = head;
    
    // Move slow one step and fast two steps at a time
    // If there's a cycle, fast will eventually catch up to slow
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        
        // If slow and fast meet, there's a cycle
        if (slow === fast) {
            return true;
        }
    }
    
    // If fast reaches the end, there's no cycle
    return false;
};
```

#### Java
```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        // Handle edge case of empty list
        if (head == null) {
            return false;
        }
        
        // Initialize slow and fast pointers
        ListNode slow = head;
        ListNode fast = head;
        
        // Move slow one step and fast two steps at a time
        // If there's a cycle, fast will eventually catch up to slow
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            
            // If slow and fast meet, there's a cycle
            if (slow == fast) {
                return true;
            }
        }
        
        // If fast reaches the end, there's no cycle
        return false;
    }
}
```
