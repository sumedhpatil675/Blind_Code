# Non-overlapping Intervals

Given an array of intervals where `intervals[i] = [starti, endi]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

## Approaches

### 1. Recursion
- **Time complexity**: O(2^n)
- **Space complexity**: O(n)

#### Python
```python
def eraseOverlapIntervals(intervals):
    intervals.sort()
    
    def dfs(i, prev):
        if i == len(intervals):
            return 0
        
        # Skip current interval
        res = dfs(i + 1, prev)
        
        # Include current interval if it doesn't overlap with previous
        if prev == -1 or intervals[prev][1] <= intervals[i][0]:
            res = max(res, 1 + dfs(i + 1, i))
            
        return res
    
    return len(intervals) - dfs(0, -1)
```

#### JavaScript
```javascript
function eraseOverlapIntervals(intervals) {
    intervals.sort((a, b) => a[0] - b[0]);
    
    function dfs(i, prev) {
        if (i === intervals.length) {
            return 0;
        }
        
        // Skip current interval
        let res = dfs(i + 1, prev);
        
        // Include current interval if it doesn't overlap with previous
        if (prev === -1 || intervals[prev][1] <= intervals[i][0]) {
            res = Math.max(res, 1 + dfs(i + 1, i));
        }
        
        return res;
    }
    
    return intervals.length - dfs(0, -1);
}
```

#### Java
```java
public int eraseOverlapIntervals(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    
    return intervals.length - dfs(intervals, 0, -1);
}

private int dfs(int[][] intervals, int i, int prev) {
    if (i == intervals.length) {
        return 0;
    }
    
    // Skip current interval
    int res = dfs(intervals, i + 1, prev);
    
    // Include current interval if it doesn't overlap with previous
    if (prev == -1 || intervals[prev][1] <= intervals[i][0]) {
        res = Math.max(res, 1 + dfs(intervals, i + 1, i));
    }
    
    return res;
}
```

### 2. Dynamic Programming (Top-Down)
- **Time complexity**: O(n²)
- **Space complexity**: O(n)

#### Python
```python
def eraseOverlapIntervals(intervals):
    intervals.sort(key=lambda x: x[1])
    n = len(intervals)
    memo = {}
    
    def dfs(i):
        if i in memo:
            return memo[i]
        
        res = 1  # Current interval is included
        for j in range(i + 1, n):
            if intervals[i][1] <= intervals[j][0]:  # No overlap
                res = max(res, 1 + dfs(j))
                
        memo[i] = res
        return res
    
    # Calculate the maximum number of non-overlapping intervals
    max_non_overlap = 0
    for i in range(n):
        max_non_overlap = max(max_non_overlap, dfs(i))
        
    return n - max_non_overlap
```

#### JavaScript
```javascript
function eraseOverlapIntervals(intervals) {
    intervals.sort((a, b) => a[1] - b[1]);
    const n = intervals.length;
    const memo = new Map();
    
    function dfs(i) {
        if (memo.has(i)) {
            return memo.get(i);
        }
        
        let res = 1;  // Current interval is included
        for (let j = i + 1; j < n; j++) {
            if (intervals[i][1] <= intervals[j][0]) {  // No overlap
                res = Math.max(res, 1 + dfs(j));
            }
        }
        
        memo.set(i, res);
        return res;
    }
    
    // Calculate the maximum number of non-overlapping intervals
    let max_non_overlap = 0;
    for (let i = 0; i < n; i++) {
        max_non_overlap = Math.max(max_non_overlap, dfs(i));
    }
    
    return n - max_non_overlap;
}
```

#### Java
```java
public int eraseOverlapIntervals(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));
    int n = intervals.length;
    Map<Integer, Integer> memo = new HashMap<>();
    
    // Calculate the maximum number of non-overlapping intervals
    int maxNonOverlap = 0;
    for (int i = 0; i < n; i++) {
        maxNonOverlap = Math.max(maxNonOverlap, dfs(intervals, i, memo));
    }
    
    return n - maxNonOverlap;
}

private int dfs(int[][] intervals, int i, Map<Integer, Integer> memo) {
    if (memo.containsKey(i)) {
        return memo.get(i);
    }
    
    int res = 1;  // Current interval is included
    for (int j = i + 1; j < intervals.length; j++) {
        if (intervals[i][1] <= intervals[j][0]) {  // No overlap
            res = Math.max(res, 1 + dfs(intervals, j, memo));
        }
    }
    
    memo.put(i, res);
    return res;
}
```

### 3. Dynamic Programming (Bottom-Up)
- **Time complexity**: O(n²)
- **Space complexity**: O(n)

#### Python
```python
def eraseOverlapIntervals(intervals):
    if not intervals:
        return 0
        
    intervals.sort(key=lambda x: x[1])
    n = len(intervals)
    dp = [1] * n
    
    for i in range(n):
        for j in range(i):
            if intervals[j][1] <= intervals[i][0]:
                dp[i] = max(dp[i], 1 + dp[j])
    
    max_non_overlapping = max(dp)
    return n - max_non_overlapping
```

#### JavaScript
```javascript
function eraseOverlapIntervals(intervals) {
    if (intervals.length === 0) {
        return 0;
    }
    
    intervals.sort((a, b) => a[1] - b[1]);
    const n = intervals.length;
    const dp = Array(n).fill(1);
    
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (intervals[j][1] <= intervals[i][0]) {
                dp[i] = Math.max(dp[i], 1 + dp[j]);
            }
        }
    }
    
    const maxNonOverlapping = Math.max(...dp);
    return n - maxNonOverlapping;
}
```

#### Java
```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) {
        return 0;
    }
    
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));
    int n = intervals.length;
    int[] dp = new int[n];
    Arrays.fill(dp, 1);
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (intervals[j][1] <= intervals[i][0]) {
                dp[i] = Math.max(dp[i], 1 + dp[j]);
            }
        }
    }
    
    int maxNonOverlapping = 0;
    for (int val : dp) {
        maxNonOverlapping = Math.max(maxNonOverlapping, val);
    }
    
    return n - maxNonOverlapping;
}
```

### 4. Greedy (Sort by End)
- **Time complexity**: O(n log n)
- **Space complexity**: O(1) or O(n) depending on the sorting algorithm

#### Python
```python
def eraseOverlapIntervals(intervals):
    if not intervals:
        return 0
        
    intervals.sort(key=lambda x: x[1])
    count = 1  # Count of non-overlapping intervals
    end = intervals[0][1]
    
    for i in range(1, len(intervals)):
        if intervals[i][0] >= end:
            count += 1
            end = intervals[i][1]
    
    return len(intervals) - count
```

#### JavaScript
```javascript
function eraseOverlapIntervals(intervals) {
    if (intervals.length === 0) {
        return 0;
    }
    
    intervals.sort((a, b) => a[1] - b[1]);
    let count = 1;  // Count of non-overlapping intervals
    let end = intervals[0][1];
    
    for (let i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= end) {
            count++;
            end = intervals[i][1];
        }
    }
    
    return intervals.length - count;
}
```

#### Java
```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) {
        return 0;
    }
    
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));
    int count = 1;  // Count of non-overlapping intervals
    int end = intervals[0][1];
    
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= end) {
            count++;
            end = intervals[i][1];
        }
    }
    
    return intervals.length - count;
}
```

### 5. Greedy (Sort by Start) ✅✅✅

- **Time complexity**: O(n log n)
- **Space complexity**: O(1) or O(n) depending on the sorting algorithm

#### Python
```python
def eraseOverlapIntervals(intervals):
    if not intervals:
        return 0
        
    intervals.sort()
    res = 0
    prevEnd = intervals[0][1]
    
    for start, end in intervals[1:]:
        if start >= prevEnd:
            prevEnd = end
        else:
            res += 1
            prevEnd = min(end, prevEnd)
    
    return res
```

#### JavaScript
```javascript
function eraseOverlapIntervals(intervals) {
    if (intervals.length === 0) {
        return 0;
    }
    
    intervals.sort((a, b) => a[0] - b[0]);
    let res = 0;
    let prevEnd = intervals[0][1];
    
    for (let i = 1; i < intervals.length; i++) {
        const [start, end] = intervals[i];
        if (start >= prevEnd) {
            prevEnd = end;
        } else {
            res++;
            prevEnd = Math.min(end, prevEnd);
        }
    }
    
    return res;
}
```

#### Java
```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) {
        return 0;
    }
    
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    int res = 0;
    int prevEnd = intervals[0][1];
    
    for (int i = 1; i < intervals.length; i++) {
        int start = intervals[i][0];
        int end = intervals[i][1];
        
        if (start >= prevEnd) {
            prevEnd = end;
        } else {
            res++;
            prevEnd = Math.min(end, prevEnd);
        }
    }
    
    return res;
}
```
