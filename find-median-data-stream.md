# Find Median from Data Stream

The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

Design a data structure that supports the following two operations:
- `addNum(int num)`: Add an integer number from the data stream to the data structure.
- `findMedian()`: Return the median of all elements so far.

## Approaches

### 1. Sorting

**Time complexity:**  
- `addNum()`: O(1)
- `findMedian()`: O(n log n)  

**Space complexity:** O(n)

#### Python
```python
class MedianFinder:
    def __init__(self):
        self.data = []
        
    def addNum(self, num: int) -> None:
        self.data.append(num)
        
    def findMedian(self) -> float:
        self.data.sort()
        n = len(self.data)
        return (self.data[n // 2] if (n & 1) else
                (self.data[n // 2] + self.data[n // 2 - 1]) / 2)
```

#### JavaScript
```javascript
/**
 * initialize your data structure here.
 */
var MedianFinder = function() {
    this.data = [];
};

/** 
 * @param {number} num
 * @return {void}
 */
MedianFinder.prototype.addNum = function(num) {
    this.data.push(num);
};

/**
 * @return {number}
 */
MedianFinder.prototype.findMedian = function() {
    this.data.sort((a, b) => a - b);
    const n = this.data.length;
    
    if (n % 2 === 1) {
        return this.data[Math.floor(n / 2)];
    } else {
        return (this.data[n / 2] + this.data[n / 2 - 1]) / 2;
    }
};
```

#### Java
```java
class MedianFinder {
    private List<Integer> data;

    /** initialize your data structure here. */
    public MedianFinder() {
        data = new ArrayList<>();
    }
    
    public void addNum(int num) {
        data.add(num);
    }
    
    public double findMedian() {
        Collections.sort(data);
        int n = data.size();
        
        if (n % 2 == 1) {
            return data.get(n / 2);
        } else {
            return (data.get(n / 2) + data.get(n / 2 - 1)) / 2.0;
        }
    }
}
```

### 2. Heap ✅

**Time complexity:**  
- `addNum()`: O(log n)
- `findMedian()`: O(1)  

**Space complexity:** O(n)

#### Python
```python
class MedianFinder:
    def __init__(self):
        # two heaps, large, small, minheap, maxheap
        # heaps should be equal size
        self.small, self.large = [], []
        
    def addNum(self, num: int) -> None:
        if self.large and num > self.large[0]:
            heapq.heappush(self.large, num)
        else:
            heapq.heappush(self.small, -1 * num)
        
        if len(self.small) > len(self.large) + 1:
            val = -1 * heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        
        if len(self.large) > len(self.small) + 1:
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -1 * val)
        
    def findMedian(self) -> float:
        if len(self.small) > len(self.large):
            return -1 * self.small[0]
        elif len(self.large) > len(self.small):
            return self.large[0]
        
        return (-1 * self.small[0] + self.large[0]) / 2.0
```

#### JavaScript
```javascript
/**
 * NumberStream class for finding the median of a data stream
 */
class NumberStream {
  constructor() {
    // Max heap to store the smaller half of the numbers
    this.maxHeap = [];
    // Min heap to store the larger half of the numbers
    this.minHeap = [];
  }

  /**
   * Adds a number into the data structure
   * @param {number} num - The number to add
   */
  add(num) {
    // Add the new number to the max heap
    this.addNumberToHeap(this.maxHeap, num, (a, b) => b - a);
    
    // Balance the heaps: move the largest number from maxHeap to minHeap
    this.addNumberToHeap(
      this.minHeap,
      this.removeTopFromHeap(this.maxHeap),
      (a, b) => a - b
    );
    
    // Ensure that maxHeap has more elements than minHeap if their sizes differ
    if (this.maxHeap.length < this.minHeap.length) {
      this.addNumberToHeap(
        this.maxHeap,
        this.removeTopFromHeap(this.minHeap),
        (a, b) => b - a
      );
    }
  }

  /**
   * Returns the median of the current data stream
   * @returns {number} The median value
   */
  getMedian() {
    // If maxHeap has more elements, the median is its top element
    if (this.maxHeap.length > this.minHeap.length) {
      return this.maxHeap[0];
    }
    // Otherwise, the median is the average of the tops of maxHeap and minHeap
    else {
      return (this.maxHeap[0] + this.minHeap[0]) * 0.5;
    }
  }

  /**
   * Utility function to add a number to a heap, maintaining the heap property
   * @param {number[]} heap - The heap array
   * @param {number} num - The number to add
   * @param {Function} comparator - The comparison function
   */
  addNumberToHeap(heap, num, comparator) {
    heap.push(num);
    let i = heap.length - 1;
    while (i > 0) {
      let parent = Math.floor((i - 1) / 2);
      if (comparator(heap[i], heap[parent]) > 0) {
        [heap[i], heap[parent]] = [heap[parent], heap[i]];
        i = parent;
      } else {
        break;
      }
    }
  }

  /**
   * Utility function to remove the top element from a heap, maintaining the heap property
   * @param {number[]} heap - The heap array
   * @returns {number} The top element
   */
  removeTopFromHeap(heap) {
    if (heap.length === 0) return NaN;
    const top = heap[0];
    const last = heap.pop();
    if (heap.length > 0 && last !== undefined) {
      heap[0] = last;
      this.heapify(heap, 0, (a, b) => (heap === this.maxHeap ? b - a : a - b));
    }
    return top;
  }

  /**
   * Utility function to maintain the heap property from the given index downwards
   * @param {number[]} heap - The heap array
   * @param {number} i - The index to start heapify from
   * @param {Function} comparator - The comparison function
   */
  heapify(heap, i, comparator) {
    const length = heap.length;
    let largest = i;
    const left = 2 * i + 1;
    const right = 2 * i + 2;
    
    if (left < length && comparator(heap[left], heap[largest]) > 0) {
      largest = left;
    }
    
    if (right < length && comparator(heap[right], heap[largest]) > 0) {
      largest = right;
    }
    
    if (largest !== i) {
      [heap[i], heap[largest]] = [heap[largest], heap[i]];
      this.heapify(heap, largest, comparator);
    }
  }
}

// Export the class for use in other files
export default NumberStream;

```

##### NumberStream Class: Finding the Median of a Data Stream

The `NumberStream` class is designed to efficiently track the median value of a continuous stream of numbers. This is a common problem in data processing where you need to find the middle value in a dynamic dataset that's constantly receiving new numbers.

#### Core Concept

The key insight of this implementation is using two heaps to partition the data:

1. **Max Heap** - Stores the smaller half of the numbers
2. **Min Heap** - Stores the larger half of the numbers

With this arrangement, the median is always accessible in O(1) time complexity.

#### ASCII Visualization

Here's how the data structure looks conceptually:

```
                Median
                   ↓
  Max Heap      |   |      Min Heap
 (smaller half) |   |    (larger half)
                |   |
  [5, 3, 1]     |   |     [8, 10, 15]
     ↑          |   |        ↑
  Largest of    |   |    Smallest of
  smaller half  |   |    larger half
```

#### Adding a Number to the Stream

When adding a new number, the algorithm follows these steps:

1. Add the number to the max heap
2. Move the largest element from max heap to min heap
3. If max heap has fewer elements than min heap, move the smallest element from min heap back to max heap

Let's visualize this with an example:

#### Initial state (empty):
```
  Max Heap: []
  Min Heap: []
```

#### After adding 10:
```
  Max Heap: [10]   <- Median is 10
  Min Heap: []
```

#### After adding 5:
```
  Add 5 to Max Heap: [10, 5]
  Move 10 to Min Heap
  
  Max Heap: [5]    <- Median is 5
  Min Heap: [10]
```

#### After adding 15:
```
  Add 15 to Max Heap: [15, 5]
  Heapify max heap: [15, 5]
  Move 15 to Min Heap
  
  Max Heap: [5]
  Min Heap: [10, 15]  
  
  Median is 7.5 (average of 5 and 10)
```

#### After adding 3:
```
  Add 3 to Max Heap: [5, 3]
  Move 5 to Min Heap
  
  Max Heap: [3]
  Min Heap: [5, 10, 15]
  
  Since Max Heap has fewer elements, move 5 back to Max Heap
  
  Max Heap: [5, 3]  <- Median is 5
  Min Heap: [10, 15]
```

#### Heap Implementation

The class uses arrays to implement the heaps, with utility functions to maintain the heap property:

##### Heap Structure for Max Heap (with array indices):
```
       [0]
       / \
    [1]   [2]
    / \   / \
  [3] [4] ...
```

#### Heap Operations

1. **addNumberToHeap**: Adds a number to a heap and bubbles it up to maintain the heap property.

```
Example: Adding 7 to Max Heap [10, 5, 3]

1. Insert at end: [10, 5, 3, 7]
2. Bubble up: Compare 7 with parent (5)
   7 > 5, so swap: [10, 7, 3, 5]
3. Compare 7 with parent (10)
   7 < 10, so stop
4. Final Max Heap: [10, 7, 3, 5]
```

2. **removeTopFromHeap**: Removes the top element, replaces it with the last element, and sifts it down.

```
Example: Removing top from Max Heap [10, 7, 3, 5]

1. Save top: 10
2. Replace with last: [5, 7, 3]
3. Sift down: 5 < 7, so swap with larger child
   [7, 5, 3]
4. Final Max Heap: [7, 5, 3]
```

3. **heapify**: Ensures the heap property is maintained by sifting an element down.

```
Example: Heapify on Max Heap [5, 7, 3] at index 0

1. Find largest among [0], [1], [2]
   5 < 7, so largest = 1
2. Swap [0] and [1]: [7, 5, 3]
3. Recursively heapify at 1
   Compare 5 and 3, 5 > 3, so stop
4. Final Max Heap: [7, 5, 3]
```

#### Calculating the Median

The median calculation is straightforward:

1. If max heap has more elements than min heap, the median is the top of max heap.
2. If both heaps have the same number of elements, the median is the average of the tops of both heaps.

#### Time and Space Complexity

- **Time Complexity**:
  - Adding a number: O(log n)
  - Getting the median: O(1)

- **Space Complexity**: O(n) where n is the number of elements in the stream.

#### Use Cases

This data structure is particularly useful for:
- Financial data analysis where you need the running median
- Traffic monitoring systems
- Any system that needs to process a continuous stream of data while maintaining the median
  

#### Java
```java
class MedianFinder {
    private PriorityQueue<Integer> small; // max heap
    private PriorityQueue<Integer> large; // min heap

    /** initialize your data structure here. */
    public MedianFinder() {
        small = new PriorityQueue<>((a, b) -> b - a); // max heap
        large = new PriorityQueue<>(); // min heap
    }
    
    public void addNum(int num) {
        if (!large.isEmpty() && num > large.peek()) {
            large.offer(num);
        } else {
            small.offer(num);
        }
        
        // Balance heaps
        if (small.size() > large.size() + 1) {
            large.offer(small.poll());
        } else if (large.size() > small.size() + 1) {
            small.offer(large.poll());
        }
    }
    
    public double findMedian() {
        if (small.size() > large.size()) {
            return small.peek();
        } else if (large.size() > small.size()) {
            return large.peek();
        }
        
        return (small.peek() + large.peek()) / 2.0;
    }
}
```
