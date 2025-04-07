# Reverse Linked List

## Problem Description
Given the head of a singly linked list, reverse the list, and return the reversed list.

## Approaches

### 1. Iterative Approach ✅✅✅

* Time complexity: O(n)
* Space complexity: O(1)

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev = None
        curr = head
        
        while curr:
            # Save next node before we change current's next pointer
            next_temp = curr.next
            
            # Reverse current node's pointer
            curr.next = prev
            
            # Move prev and curr one step forward
            prev = curr
            curr = next_temp
            
        # After the loop, prev is pointing to the new head
        return prev
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
var reverseList = function(head) {
    let prev = null;
    let curr = head;
    
    while (curr) {
        // Save next node before we change current's next pointer
        const nextTemp = curr.next;
        
        // Reverse current node's pointer
        curr.next = prev;
        
        // Move prev and curr one step forward
        prev = curr;
        curr = nextTemp;
    }
    
    // After the loop, prev is pointing to the new head
    return prev;
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
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        
        while (curr != null) {
            // Save next node before we change current's next pointer
            ListNode nextTemp = curr.next;
            
            // Reverse current node's pointer
            curr.next = prev;
            
            // Move prev and curr one step forward
            prev = curr;
            curr = nextTemp;
        }
        
        // After the loop, prev is pointing to the new head
        return prev;
    }
}
```

### 2. Recursive Approach
* Time complexity: O(n)
* Space complexity: O(n) due to the call stack

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        # Base case: empty list or list with only one node
        if not head or not head.next:
            return head
        
        # Recursive case: reverse the rest of the list after head
        # newHead will be the new head of the reversed list
        newHead = self.reverseList(head.next)
        
        # Make head.next point back to head
        head.next.next = head
        
        # Break the original forward link
        head.next = None
        
        return newHead
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
var reverseList = function(head) {
    // Base case: empty list or list with only one node
    if (!head || !head.next) {
        return head;
    }
    
    // Recursive case: reverse the rest of the list after head
    // newHead will be the new head of the reversed list
    const newHead = reverseList(head.next);
    
    // Make head.next point back to head
    head.next.next = head;
    
    // Break the original forward link
    head.next = null;
    
    return newHead;
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
    public ListNode reverseList(ListNode head) {
        // Base case: empty list or list with only one node
        if (head == null || head.next == null) {
            return head;
        }
        
        // Recursive case: reverse the rest of the list after head
        // newHead will be the new head of the reversed list
        ListNode newHead = reverseList(head.next);
        
        // Make head.next point back to head
        head.next.next = head;
        
        // Break the original forward link
        head.next = null;
        
        return newHead;
    }
}
```
