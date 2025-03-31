# Reorder List

## Problem Description
You are given the head of a singly linked-list. The list can be represented as:
```
L0 → L1 → ... → Ln-1 → Ln
```

Reorder the list to be on the following form:
```
L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...
```

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

## Approaches

### 1. Brute Force (Using Array)
* Time complexity: O(n)
* Space complexity: O(n)

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reorderList(self, head: ListNode) -> None:
        if not head:
            return
        
        # Store all nodes in an array
        nodes = []
        curr = head
        while curr:
            nodes.append(curr)
            curr = curr.next
        
        # Reorder the list using two pointers
        i, j = 0, len(nodes) - 1
        while i < j:
            # Connect first node to last node
            nodes[i].next = nodes[j]
            i += 1
            
            # Connect last node to second node (if there is one)
            if i < j:
                nodes[j].next = nodes[i]
                j -= 1
        
        # Make sure the last node's next points to None
        nodes[i].next = None
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
 * @return {void} Do not return anything, modify head in-place instead.
 */
var reorderList = function(head) {
    if (!head) {
        return;
    }
    
    // Store all nodes in an array
    const nodes = [];
    let curr = head;
    while (curr) {
        nodes.push(curr);
        curr = curr.next;
    }
    
    // Reorder the list using two pointers
    let i = 0, j = nodes.length - 1;
    while (i < j) {
        // Connect first node to last node
        nodes[i].next = nodes[j];
        i++;
        
        // Connect last node to second node (if there is one)
        if (i < j) {
            nodes[j].next = nodes[i];
            j--;
        }
    }
    
    // Make sure the last node's next points to null
    nodes[i].next = null;
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
    public void reorderList(ListNode head) {
        if (head == null) {
            return;
        }
        
        // Store all nodes in an array
        List<ListNode> nodes = new ArrayList<>();
        ListNode curr = head;
        while (curr != null) {
            nodes.add(curr);
            curr = curr.next;
        }
        
        // Reorder the list using two pointers
        int i = 0, j = nodes.size() - 1;
        while (i < j) {
            // Connect first node to last node
            nodes.get(i).next = nodes.get(j);
            i++;
            
            // Connect last node to second node (if there is one)
            if (i < j) {
                nodes.get(j).next = nodes.get(i);
                j--;
            }
        }
        
        // Make sure the last node's next points to null
        nodes.get(i).next = null;
    }
}
```

### 2. Three-Step Approach (Find Middle, Reverse Second Half, Merge)
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
    def reorderList(self, head: ListNode) -> None:
        if not head:
            return
        
        # Step 1: Find the middle of the linked list
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            
        # Step 2: Reverse the second half of the linked list
        # Now slow points to the middle node
        prev = None
        curr = slow
        while curr:
            next_temp = curr.next
            curr.next = prev
            prev = curr
            curr = next_temp
            
        # Step 3: Merge the first half and the reversed second half
        # prev now points to the head of reversed second half
        first, second = head, prev
        while second.next:
            # Save next nodes
            first_next = first.next
            second_next = second.next
            
            # Connect nodes in the reordered pattern
            first.next = second
            second.next = first_next
            
            # Move pointers forward
            first = first_next
            second = second_next
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
 * @return {void} Do not return anything, modify head in-place instead.
 */
var reorderList = function(head) {
    if (!head) {
        return;
    }
    
    // Step 1: Find the middle of the linked list
    let slow = head;
    let fast = head;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Step 2: Reverse the second half of the linked list
    // Now slow points to the middle node
    let prev = null;
    let curr = slow;
    while (curr) {
        let nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    
    // Step 3: Merge the first half and the reversed second half
    // prev now points to the head of reversed second half
    let first = head;
    let second = prev;
    while (second.next) {
        // Save next nodes
        let firstNext = first.next;
        let secondNext = second.next;
        
        // Connect nodes in the reordered pattern
        first.next = second;
        second.next = firstNext;
        
        // Move pointers forward
        first = firstNext;
        second = secondNext;
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
    public void reorderList(ListNode head) {
        if (head == null) {
            return;
        }
        
        // Step 1: Find the middle of the linked list
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        // Step 2: Reverse the second half of the linked list
        // Now slow points to the middle node
        ListNode prev = null;
        ListNode curr = slow;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        
        // Step 3: Merge the first half and the reversed second half
        // prev now points to the head of reversed second half
        ListNode first = head;
        ListNode second = prev;
        while (second.next != null) {
            // Save next nodes
            ListNode firstNext = first.next;
            ListNode secondNext = second.next;
            
            // Connect nodes in the reordered pattern
            first.next = second;
            second.next = firstNext;
            
            // Move pointers forward
            first = firstNext;
            second = secondNext;
        }
    }
}
```