# Insert Interval

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the ith interval and intervals is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into intervals such that intervals is still sorted in ascending order by `starti` and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

## Approaches

### 1. Linear Search
- **Time complexity**: O(n)
- **Space complexity**: O(n)

#### Python
```python
def insert(intervals, newInterval):
    n = len(intervals)
    i = 0
    res = []
    
    # Add all intervals that come before newInterval
    while i < n and intervals[i][1] < newInterval[0]:
        res.append(intervals[i])
        i += 1
    
    # Merge overlapping intervals
    while i < n and newInterval[1] >= intervals[i][0]:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
        i += 1
    
    # Add the merged interval
    res.append(newInterval)
    
    # Add all intervals that come after newInterval
    while i < n:
        res.append(intervals[i])
        i += 1
    
    return res
```

#### JavaScript
```javascript
function insert(intervals, newInterval) {
    const n = intervals.length;
    let i = 0;
    const res = [];
    
    // Add all intervals that come before newInterval
    while (i < n && intervals[i][1] < newInterval[0]) {
        res.push(intervals[i]);
        i++;
    }
    
    // Merge overlapping intervals
    while (i < n && newInterval[1] >= intervals[i][0]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    
    // Add the merged interval
    res.push(newInterval);
    
    // Add all intervals that come after newInterval
    while (i < n) {
        res.push(intervals[i]);
        i++;
    }
    
    return res;
}
```

#### Java
```java
public int[][] insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new ArrayList<>();
    int i = 0, n = intervals.length;
    
    // Add all intervals that come before newInterval
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.add(intervals[i]);
        i++;
    }
    
    // Merge overlapping intervals
    while (i < n && newInterval[1] >= intervals[i][0]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    
    // Add the merged interval
    result.add(newInterval);
    
    // Add all intervals that come after newInterval
    while (i < n) {
        result.add(intervals[i]);
        i++;
    }
    
    return result.toArray(new int[result.size()][]);
}
```

### 2. Binary Search
- **Time complexity**: O(n)
- **Space complexity**: O(n)

#### Python
```python
def insert(intervals, newInterval):
    if not intervals:
        return [newInterval]
    
    n = len(intervals)
    target = newInterval[0]
    left, right = 0, n - 1
    
    # Binary search to find the position to insert
    while left <= right:
        mid = (left + right) // 2
        if intervals[mid][0] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    # Insert the new interval at the found position
    intervals.insert(left, newInterval)
    
    # Merge overlapping intervals
    res = []
    for interval in intervals:
        if not res or res[-1][1] < interval[0]:
            res.append(interval)
        else:
            res[-1][1] = max(res[-1][1], interval[1])
    
    return res
```

#### JavaScript
```javascript
function insert(intervals, newInterval) {
    if (intervals.length === 0) {
        return [newInterval];
    }
    
    const n = intervals.length;
    const target = newInterval[0];
    let left = 0, right = n - 1;
    
    // Binary search to find the position to insert
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (intervals[mid][0] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    // Insert the new interval at the found position
    intervals.splice(left, 0, newInterval);
    
    // Merge overlapping intervals
    const res = [];
    for (const interval of intervals) {
        if (res.length === 0 || res[res.length - 1][1] < interval[0]) {
            res.push(interval);
        } else {
            res[res.length - 1][1] = Math.max(res[res.length - 1][1], interval[1]);
        }
    }
    
    return res;
}
```

#### Java
```java
public int[][] insert(int[][] intervals, int[] newInterval) {
    if (intervals.length == 0) {
        return new int[][] {newInterval};
    }
    
    int n = intervals.length;
    int target = newInterval[0];
    int left = 0, right = n - 1;
    
    // Binary search to find the position to insert
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (intervals[mid][0] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    // Insert the new interval at the found position
    List<int[]> list = new ArrayList<>();
    for (int i = 0; i < left; i++) {
        list.add(intervals[i]);
    }
    list.add(newInterval);
    for (int i = left; i < n; i++) {
        list.add(intervals[i]);
    }
    
    // Merge overlapping intervals
    List<int[]> result = new ArrayList<>();
    for (int[] interval : list) {
        if (result.isEmpty() || result.get(result.size() - 1)[1] < interval[0]) {
            result.add(interval);
        } else {
            result.get(result.size() - 1)[1] = Math.max(result.get(result.size() - 1)[1], interval[1]);
        }
    }
    
    return result.toArray(new int[result.size()][]);
}
```

### 3. Greedy (One-pass)
- **Time complexity**: O(n)
- **Space complexity**: O(n)

#### Python
```python
def insert(intervals, newInterval):
    res = []
    
    for i, interval in enumerate(intervals):
        # If new interval comes before current interval
        if newInterval[1] < interval[0]:
            res.append(newInterval)
            return res + intervals[i:]
            
        # If current interval comes before new interval
        elif interval[1] < newInterval[0]:
            res.append(interval)
            
        # If there is overlap, merge them
        else:
            newInterval = [
                min(newInterval[0], interval[0]),
                max(newInterval[1], interval[1])
            ]
    
    # Add the final merged interval
    res.append(newInterval)
    return res
```

#### JavaScript
```javascript
function insert(intervals, newInterval) {
    const res = [];
    
    for (let i = 0; i < intervals.length; i++) {
        const interval = intervals[i];
        
        // If new interval comes before current interval
        if (newInterval[1] < interval[0]) {
            res.push(newInterval);
            return [...res, ...intervals.slice(i)];
        }
        
        // If current interval comes before new interval
        else if (interval[1] < newInterval[0]) {
            res.push(interval);
        }
        
        // If there is overlap, merge them
        else {
            newInterval = [
                Math.min(newInterval[0], interval[0]),
                Math.max(newInterval[1], interval[1])
            ];
        }
    }
    
    // Add the final merged interval
    res.push(newInterval);
    return res;
}
```

#### Java
```java
public int[][] insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new ArrayList<>();
    
    for (int i = 0; i < intervals.length; i++) {
        // If new interval comes before current interval
        if (newInterval[1] < intervals[i][0]) {
            result.add(newInterval);
            for (int j = i; j < intervals.length; j++) {
                result.add(intervals[j]);
            }
            return result.toArray(new int[result.size()][]);
        }
        
        // If current interval comes before new interval
        else if (intervals[i][1] < newInterval[0]) {
            result.add(intervals[i]);
        }
        
        // If there is overlap, merge them
        else {
            newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
            newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        }
    }
    
    // Add the final merged interval
    result.add(newInterval);
    return result.toArray(new int[result.size()][]);
}
```