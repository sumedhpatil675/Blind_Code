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

### 3. Greedy (One-pass) ✅
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

#### Insert Interval Algorithm Explanation

This algorithm inserts a new interval into a list of non-overlapping sorted intervals, and merges any overlapping intervals.

### Algorithm Overview

1. Create an empty result list
2. Iterate through the existing intervals:
   - If the new interval comes entirely before the current interval, add the new interval to the result, then add all remaining intervals and return
   - If the new interval comes entirely after the current interval, add the current interval to the result
   - If the new interval overlaps with the current interval, merge them into a new interval and continue
3. After the loop, append the (possibly merged) new interval to the result
4. Return the result

### Visual Explanation with ASCII Diagrams

Let's step through an example:
```
intervals = [[1,3], [6,9]], newInterval = [2,5]
```

#### Initial state:
```
Timeline: 0----5----10
Intervals:  [1-3]     [6-9]
newInterval:    [2-5]
res = []
```

##### Iteration 1: intervals[0] = [1,3]
Compare with newInterval [2,5]

Since newInterval[0] = 2 ≤ intervals[0][1] = 3 AND newInterval[1] = 5 ≥ intervals[0][0] = 1, they overlap.

Merge them: newInterval = [min(1,2), max(3,5)] = [1,5]

```
Timeline: 0----5----10
Intervals:  [1-3]     [6-9]
newInterval: [1---5]
res = []
```

##### Iteration 2: intervals[1] = [6,9]
Compare with newInterval [1,5]

Since newInterval[1] = 5 < intervals[1][0] = 6, the new interval comes entirely before this interval.

Add newInterval to res, then add all remaining intervals:

```
Timeline: 0----5----10
Intervals:  [1-3]     [6-9]
                      ↑
                      |
                      +--- Add to result
newInterval: [1---5]
              ↑
              |
              +--- Add to result
res = [[1,5], [6,9]]
```

##### Final result:
```
res = [[1,5], [6,9]]
```

### Different Scenarios

##### Scenario 1: New interval comes before current interval
```
intervals = [[3,4]], newInterval = [1,2]
```

```
Timeline: 0----5
Intervals:     [3-4]
newInterval: [1-2]
```

Since 2 < 3, we add newInterval to result, then add all remaining intervals:
```
res = [[1,2], [3,4]]
```

##### Scenario 2: New interval comes after current interval
```
intervals = [[3,4]], newInterval = [5,6]
```

```
Timeline: 0----5----
Intervals:  [3-4]
newInterval:      [5-6]
```

First iteration: Since 5 > 4, we add intervals[0] to result:
```
res = [[3,4]]
```

After loop: Add the newInterval to result:
```
res = [[3,4], [5,6]]
```

##### Scenario 3: Overlapping intervals in the middle
```
intervals = [[1,2], [5,6]], newInterval = [3,4]
```

```
Timeline: 0----5----
Intervals:  [1-2]    [5-6]
newInterval:    [3-4]
```

First iteration: 3 > 2, add intervals[0] to result:
```
res = [[1,2]]
```

Second iteration: 4 < 5, add newInterval to result, then add all remaining intervals:
```
res = [[1,2], [3,4], [5,6]]
```

##### Scenario 4: New interval that overlaps multiple intervals
```
intervals = [[1,3], [5,7], [9,11]], newInterval = [2,10]
```

```
Timeline: 0----5----10---
Intervals:  [1-3] [5-7] [9-11]
newInterval:   [2---------10]
```

First iteration: Overlap with [1,3], merge to get newInterval = [1,10]
Second iteration: Overlap with [5,7], merge to get newInterval = [1,10]
Third iteration: Overlap with [9,11], merge to get newInterval = [1,11]

After loop: Add the merged interval:
```
res = [[1,11]]
```

#### Key Insights

1. The algorithm handles three distinct cases:
   - New interval comes before current interval
   - New interval comes after current interval
   - New interval overlaps with current interval

2. The newInterval can get progressively merged with multiple intervals in the list

3. The early return in the first case (newInterval[1] < intervals[i][0]) is an optimization that avoids processing the rest of the intervals when we know where the new interval belongs

4. The result list is built incrementally, ensuring intervals remain in sorted order
   

#### JavaScript
```javascript
function insert(intervals, newInterval) {
    let res = []
    for(let i=0;i<intervals.length;i++)
    {
      let currentInterval = intervals[i]
      let currentIntervalStart = currentInterval[0]
      let currentIntervalEnd = currentInterval[1]

      let newIntervalStart = newInterval[0]
      let newIntervalEnd = newInterval[1]

      if(newIntervalEnd<currentIntervalStart)
      {
        res.push(newInterval)
        return [...res,...intervals.slice(i)]
      }
      else if(newIntervalStart>currentIntervalEnd)
      {
        res.push(intervals[i])
      }else 
      {
        newInterval = [Math.min(currentIntervalStart,newIntervalStart),Math.max(currentIntervalEnd,newIntervalEnd)]
      }

    }
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
