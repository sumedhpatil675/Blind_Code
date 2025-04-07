# Top K Frequent Elements

## Problem Description
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

## Approaches

### 1. Sorting
* Time complexity: O(n log n)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = {}
        for num in nums:
            count[num] = 1 + count.get(num, 0)
            
        # Sort by frequency (count value)
        arr = []
        for num, cnt in count.items():
            arr.append([cnt, num])
            
        arr.sort(reverse=True)  # Sort in descending order of frequency
        
        res = []
        for i in range(k):
            res.append(arr[i][1])
            
        return res
```

#### JavaScript
```javascript
var topKFrequent = function(nums, k) {
    const count = {};
    
    // Count frequencies
    for (const num of nums) {
        count[num] = (count[num] || 0) + 1;
    }
    
    // Create array of [num, frequency] pairs
    const arr = [];
    for (const num in count) {
        arr.push([parseInt(num), count[num]]);
    }
    
    // Sort by frequency (descending)
    arr.sort((a, b) => b[1] - a[1]);
    
    // Extract top k elements
    const result = [];
    for (let i = 0; i < k; i++) {
        result.push(arr[i][0]);
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // Count frequencies
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) {
            count.put(num, count.getOrDefault(num, 0) + 1);
        }
        
        // Create list of [num, frequency] pairs
        List<int[]> freqPairs = new ArrayList<>();
        for (Map.Entry<Integer, Integer> entry : count.entrySet()) {
            freqPairs.add(new int[] {entry.getKey(), entry.getValue()});
        }
        
        // Sort by frequency (descending)
        Collections.sort(freqPairs, (a, b) -> b[1] - a[1]);
        
        // Extract top k elements
        int[] result = new int[k];
        for (int i = 0; i < k; i++) {
            result[i] = freqPairs.get(i)[0];
        }
        
        return result;
    }
}
```

### 2. Heap (Priority Queue)
* Time complexity: O(n log k)
* Space complexity: O(n + k)

#### Python
```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = {}
        for num in nums:
            count[num] = 1 + count.get(num, 0)
        
        heap = []
        # Use min heap to keep track of k most frequent elements
        for num, freq in count.items():
            heapq.heappush(heap, (freq, num))
            if len(heap) > k:
                heapq.heappop(heap)
        
        res = []
        while heap:
            res.append(heapq.heappop(heap)[1])
            
        return res[::-1]  # Reverse to get from most to least frequent
```

#### JavaScript
```javascript
var topKFrequent = function(nums, k) {
    const count = {};
    
    // Count frequencies
    for (const num of nums) {
        count[num] = (count[num] || 0) + 1;
    }
    
    // Using a min heap
    const minHeap = new MinPriorityQueue();
    
    for (const num in count) {
        const freq = count[num];
        minHeap.enqueue(parseInt(num), freq);
        if (minHeap.size() > k) {
            minHeap.dequeue();
        }
    }
    
    // Extract elements from the heap
    const result = [];
    while (minHeap.size() > 0) {
        result.push(minHeap.dequeue().element);
    }
    
    return result.reverse();  // Reverse to get most frequent first
};

// Note: This is a simplified implementation. In LeetCode, you would use their
// provided PriorityQueue implementation or implement your own.
```

#### Java
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // Count frequencies
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) {
            count.put(num, count.getOrDefault(num, 0) + 1);
        }
        
        // Use a min heap of size k
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        
        for (Map.Entry<Integer, Integer> entry : count.entrySet()) {
            minHeap.offer(new int[] {entry.getKey(), entry.getValue()});
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
        
        // Extract elements from the heap
        int[] result = new int[k];
        for (int i = k - 1; i >= 0; i--) {
            result[i] = minHeap.poll()[0];
        }
        
        return result;
    }
}
```

### 3. Bucket Sort ✅✅✅

* Time complexity: O(n)
* Space complexity: O(n)

#### Python
```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = {}
        freq = [[] for i in range(len(nums) + 1)]
        
        # Count frequencies
        for num in nums:
            count[num] = 1 + count.get(num, 0)
        
        # Group numbers by frequency
        for num, c in count.items():
            freq[c].append(num)
        
        # Build result by taking most frequent elements
        res = []
        for i in range(len(freq) - 1, 0, -1):
            for num in freq[i]:
                res.append(num)
                if len(res) == k:
                    return res
```

#### JavaScript
```javascript
var topKFrequent = function(nums, k) {
    const count = {};
    
    // Count frequencies
    for (const num of nums) {
        count[num] = (count[num] || 0) + 1;
    }
    
    // Group numbers by frequency
    const freq = Array(nums.length + 1).fill().map(() => []);
    for (const num in count) {
        const frequency = count[num];
        freq[frequency].push(parseInt(num));
    }
    
    // Build result by taking most frequent elements
    const result = [];
    
    for (let i = freq.length - 1; i >= 0; i--) {
        for (const num of freq[i]) {
            result.push(num);
            if (result.length === k) {
                return result;
            }
        }
    }
    
    return result;
};
```

#### Java
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // Count frequencies
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) {
            count.put(num, count.getOrDefault(num, 0) + 1);
        }
        
        // Group numbers by frequency
        List<Integer>[] buckets = new List[nums.length + 1];
        for (int i = 0; i <= nums.length; i++) {
            buckets[i] = new ArrayList<>();
        }
        
        for (Map.Entry<Integer, Integer> entry : count.entrySet()) {
            int num = entry.getKey();
            int frequency = entry.getValue();
            buckets[frequency].add(num);
        }
        
        // Build result by taking most frequent elements
        int[] result = new int[k];
        int idx = 0;
        
        for (int i = buckets.length - 1; i >= 0; i--) {
            for (int num : buckets[i]) {
                result[idx++] = num;
                if (idx == k) {
                    return result;
                }
            }
        }
        
        return result;
    }
}
```
