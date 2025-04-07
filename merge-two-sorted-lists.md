# Merge Two Sorted Lists

## Problem Description
You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

## Approaches

### 1. Iterative Approach ✅✅✅

* Time complexity: O(n + m) where n and m are the lengths of the two lists
* Space complexity: O(1) because we are reusing the original nodes

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: ListNode, list2: ListNode) -> ListNode:
        # Create a dummy node to simplify handling the merged list's head
        dummy = ListNode()
        current = dummy
        
        # Traverse both lists
        while list1 and list2:
            # Compare values and link the smaller one
            if list1.val < list2.val:
                current.next = list1
                list1 = list1.next
            else:
                current.next = list2
                list2 = list2.next
                
            current = current.next
        
        # Link any remaining nodes
        current.next = list1 if list1 else list2
        
        # Return the head of the merged list (skip the dummy node)
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
var mergeTwoLists = function(list1, list2) {
    // Create a dummy node to simplify handling the merged list's head
    const dummy = new ListNode();
    let current = dummy;
    
    // Traverse both lists
    while (list1 && list2) {
        // Compare values and link the smaller one
        if (list1.val < list2.val) {
            current.next = list1;
            list1 = list1.next;
        } else {
            current.next = list2;
            list2 = list2.next;
        }
        
        current = current.next;
    }
    
    // Link any remaining nodes
    current.next = list1 || list2;
    
    // Return the head of the merged list (skip the dummy node)
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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // Create a dummy node to simplify handling the merged list's head
        ListNode dummy = new ListNode();
        ListNode current = dummy;
        
        // Traverse both lists
        while (list1 != null && list2 != null) {
            // Compare values and link the smaller one
            if (list1.val < list2.val) {
                current.next = list1;
                list1 = list1.next;
            } else {
                current.next = list2;
                list2 = list2.next;
            }
            
            current = current.next;
        }
        
        // Link any remaining nodes
        current.next = (list1 != null) ? list1 : list2;
        
        // Return the head of the merged list (skip the dummy node)
        return dummy.next;
    }
}
```

### 2. Recursive Approach
* Time complexity: O(n + m) where n and m are the lengths of the two lists
* Space complexity: O(n + m) due to the recursive call stack

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: ListNode, list2: ListNode) -> ListNode:
        # Base cases
        if not list1:
            return list2
        if not list2:
            return list1
        
        # Recursive case: Compare values and decide which node becomes the head
        if list1.val <= list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2
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
var mergeTwoLists = function(list1, list2) {
    // Base cases
    if (!list1) {
        return list2;
    }
    if (!list2) {
        return list1;
    }
    
    // Recursive case: Compare values and decide which node becomes the head
    if (list1.val <= list2.val) {
        list1.next = mergeTwoLists(list1.next, list2);
        return list1;
    } else {
        list2.next = mergeTwoLists(list1, list2.next);
        return list2;
    }
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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // Base cases
        if (list1 == null) {
            return list2;
        }
        if (list2 == null) {
            return list1;
        }
        
        // Recursive case: Compare values and decide which node becomes the head
        if (list1.val <= list2.val) {
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        } else {
            list2.next = mergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```
