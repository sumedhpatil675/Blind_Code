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

### 2. Heap

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
 * initialize your data structure here.
 */
var MedianFinder = function() {
    this.small = new MinPriorityQueue({ priority: x => -x }); // Max heap using negation
    this.large = new MinPriorityQueue(); // Min heap
};

/** 
 * @param {number} num
 * @return {void}
 */
MedianFinder.prototype.addNum = function(num) {
    // Add to the appropriate heap
    if (this.large.size() > 0 && num > this.large.front().element) {
        this.large.enqueue(num);
    } else {
        this.small.enqueue(-num); // Insert as negative to simulate max heap
    }
    
    // Balance heaps
    if (this.small.size() > this.large.size() + 1) {
        const val = -this.small.dequeue().element; // Convert back to positive
        this.large.enqueue(val);
    }
    
    if (this.large.size() > this.small.size() + 1) {
        const val = this.large.dequeue().element;
        this.small.enqueue(-val); // Convert to negative for max heap
    }
};

/**
 * @return {number}
 */
MedianFinder.prototype.findMedian = function() {
    if (this.small.size() > this.large.size()) {
        return -this.small.front().element; // Convert back to positive
    } else if (this.large.size() > this.small.size()) {
        return this.large.front().element;
    }
    
    return (-this.small.front().element + this.large.front().element) / 2;
};
```

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