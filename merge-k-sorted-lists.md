# Merge K Sorted Lists

## Problem Description
You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

## Approaches

### 1. Brute Force Approach
* Time complexity: O(N log N) where N is the total number of nodes
* Space complexity: O(N)

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        # Collect all values from the linked lists
        nodes = []
        for l in lists:
            while l:
                nodes.append(l.val)
                l = l.next
                
        # Sort the values
        nodes.sort()
        
        # Create a new linked list with the sorted values
        dummy = ListNode(0)
        curr = dummy
        
        for val in nodes:
            curr.next = ListNode(val)
            curr = curr.next
            
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
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function(lists) {
    // Collect all values from the linked lists
    const nodes = [];
    
    for (const list of lists) {
        let current = list;
        while (current) {
            nodes.push(current.val);
            current = current.next;
        }
    }
    
    // Sort the values
    nodes.sort((a, b) => a - b);
    
    // Create a new linked list with the sorted values
    const dummy = new ListNode(0);
    let current = dummy;
    
    for (const val of nodes) {
        current.next = new ListNode(val);
        current = current.next;
    }
    
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
    public ListNode mergeKLists(ListNode[] lists) {
        // Collect all values from the linked lists
        List<Integer> nodes = new ArrayList<>();
        
        for (ListNode list : lists) {
            ListNode current = list;
            while (current != null) {
                nodes.add(current.val);
                current = current.next;
            }
        }
        
        // Sort the values
        Collections.sort(nodes);
        
        // Create a new linked list with the sorted values
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        
        for (int val : nodes) {
            current.next = new ListNode(val);
            current = current.next;
        }
        
        return dummy.next;
    }
}
```

### 2. Min Heap (Priority Queue) Approach
* Time complexity: O(N log k) where N is the total number of nodes and k is the number of linked lists
* Space complexity: O(k) for the priority queue

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
import heapq

class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        # Create a dummy node for the result list
        dummy = ListNode(0)
        curr = dummy
        
        # Initialize the min heap
        # We need to use a tuple with a counter to handle equal values
        # because ListNode doesn't have built-in comparison methods
        heap = []
        counter = 0
        
        # Add the first node from each list to the heap
        for l in lists:
            if l:
                # Push (value, counter, node) to the heap
                heapq.heappush(heap, (l.val, counter, l))
                counter += 1
        
        # Process the heap until it's empty
        while heap:
            # Pop the node with the smallest value
            val, _, node = heapq.heappop(heap)
            
            # Add this node to the result list
            curr.next = node
            curr = curr.next
            
            # If this node has a next node, add it to the heap
            if node.next:
                heapq.heappush(heap, (node.next.val, counter, node.next))
                counter += 1
                
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
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function(lists) {
    // Create a dummy node for the result list
    const dummy = new ListNode(0);
    let curr = dummy;
    
    // Create a min heap (using a simple array and sort)
    const heap = [];
    
    // Add the first node from each list to the heap
    for (const list of lists) {
        if (list) {
            heap.push(list);
        }
    }
    
    // While there are nodes in the heap
    while (heap.length > 0) {
        // Sort the heap by node values
        heap.sort((a, b) => a.val - b.val);
        
        // Take the node with the smallest value
        const node = heap.shift();
        
        // Add this node to the result list
        curr.next = node;
        curr = curr.next;
        
        // If this node has a next node, add it to the heap
        if (node.next) {
            heap.push(node.next);
        }
    }
    
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
    public ListNode mergeKLists(ListNode[] lists) {
        // Create a dummy node for the result list
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        
        // Create a min heap based on node values
        PriorityQueue<ListNode> minHeap = new PriorityQueue<>((a, b) -> a.val - b.val);
        
        // Add the first node from each list to the heap
        for (ListNode list : lists) {
            if (list != null) {
                minHeap.offer(list);
            }
        }
        
        // Process the heap until it's empty
        while (!minHeap.isEmpty()) {
            // Poll the node with the smallest value
            ListNode node = minHeap.poll();
            
            // Add this node to the result list
            curr.next = node;
            curr = curr.next;
            
            // If this node has a next node, add it to the heap
            if (node.next != null) {
                minHeap.offer(node.next);
            }
        }
        
        return dummy.next;
    }
}
```

### 3. Divide and Conquer Approach
* Time complexity: O(N log k) where N is the total number of nodes and k is the number of linked lists
* Space complexity: O(log k) for the recursion stack

#### Python
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        # Base case
        if not lists:
            return None
        if len(lists) == 1:
            return lists[0]
        
        # Recursive case: divide and conquer
        mid = len(lists) // 2
        left = self.mergeKLists(lists[:mid])
        right = self.mergeKLists(lists[mid:])
        
        # Merge the two sorted lists
        return self.mergeTwoLists(left, right)
    
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        # Create a dummy node for the result list
        dummy = ListNode(0)
        curr = dummy
        
        # Merge the two lists
        while l1 and l2:
            if l1.val < l2.val:
                curr.next = l1
                l1 = l1.next
            else:
                curr.next = l2
                l2 = l2.next
            curr = curr.next
        
        # Link any remaining nodes
        curr.next = l1 if l1 else l2
        
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
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function(lists) {
    // Base case
    if (lists.length === 0) {
        return null;
    }
    if (lists.length === 1) {
        return lists[0];
    }
    
    // Recursive case: divide and conquer
    const mid = Math.floor(lists.length / 2);
    const left = mergeKLists(lists.slice(0, mid));
    const right = mergeKLists(lists.slice(mid));
    
    // Merge the two sorted lists
    return mergeTwoLists(left, right);
};

// Helper function to merge two sorted lists
var mergeTwoLists = function(l1, l2) {
    // Create a dummy node for the result list
    const dummy = new ListNode(0);
    let curr = dummy;
    
    // Merge the two lists
    while (l1 && l2) {
        if (l1.val < l2.val) {
            curr.next = l1;
            l1 = l1.next;
        } else {
            curr.next = l2;
            l2 = l2.next;
        }
        curr = curr.next;
    }
    
    // Link any remaining nodes
    curr.next = l1 || l2;
    
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
    public ListNode mergeKLists(ListNode[] lists) {
        // Base case
        if (lists.length == 0) {
            return null;
        }
        if (lists.length == 1) {
            return lists[0];
        }
        
        // Apply divide and conquer approach
        return mergeKListsHelper(lists, 0, lists.length - 1);
    }
    
    private ListNode mergeKListsHelper(ListNode[] lists, int start, int end) {
        // Base case: only one list in this subproblem
        if (start == end) {
            return lists[start];
        }
        
        // Recursive case: divide the problem
        int mid = start + (end - start) / 2;
        ListNode left = mergeKListsHelper(lists, start, mid);
        ListNode right = mergeKListsHelper(lists, mid + 1, end);
        
        // Merge the two sorted lists
        return mergeTwoLists(left, right);
    }
    
    private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // Create a dummy node for the result list
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        
        // Merge the two lists
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                curr.next = l1;
                l1 = l1.next;
            } else {
                curr.next = l2;
                l2 = l2.next;
            }
            curr = curr.next;
        }
        
        // Link any remaining nodes
        curr.next = (l1 != null) ? l1 : l2;
        
        return dummy.next;
    }
}
```