# Merge Intervals

Given an array of intervals where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

## Approaches

### 1. Sorting ✅✅✅
 
- **Time complexity**: O(n log n)
- **Space complexity**: O(n) depending on the sorting algorithm

#### Python
```python
def merge(intervals):
    intervals.sort(key=lambda pair: pair[0])
    output = [intervals[0]]
    
    for start, end in intervals[1:]:
        lastEnd = output[-1][1]
        
        if start <= lastEnd:
            output[-1][1] = max(lastEnd, end)
        else:
            output.append([start, end])
    
    return output
```

## Algorithm Overview

The merge intervals algorithm takes a list of intervals (each represented as [start, end]) and combines any overlapping intervals.

The algorithm works as follows:

1. Sort all intervals by their start time
2. Initialize the output with the first interval
3. For each remaining interval:
   - If it overlaps with the last merged interval, combine them
   - If not, add it as a new interval to the output

## Visual Explanation with ASCII Diagrams

Let's use the example: `intervals = [[1,3], [2,6], [8,10], [15,18]]`

### Step 1: Sort intervals

Our intervals are already sorted by start time:
```
intervals = [[1,3], [2,6], [8,10], [15,18]]
```

### Step 2: Initialize output with first interval

```
Timeline: 0----5----10----15----20
Interval:  [1-3]
Output:    [1-3]
```

### Step 3: Process second interval [2,6]

We compare the start of this interval (2) with the end of our last output interval (3).
Since 2 ≤ 3, they overlap!

We merge them by keeping the earliest start and the latest end:

```
Timeline: 0----5----10----15----20
Interval:        [2----6]
Output:    [1--------6]
           ↑         ↑
           |         |
           |         +--- Extend to include new end
           +--- Keep original start
```

### Step 4: Process third interval [8,10]

We compare the start of this interval (8) with the end of our last output interval (6).
Since 8 > 6, they don't overlap!

We add this as a new interval to our output:

```
Timeline: 0----5----10----15----20
Interval:             [8-10]
Output:    [1--------6]
                       [8-10]
                       ↑
                       |
                       +--- Add as new separate interval
```

### Step 5: Process fourth interval [15,18]

We compare the start of this interval (15) with the end of our last output interval (10).
Since 15 > 10, they don't overlap!

We add this as a new interval to our output:

```
Timeline: 0----5----10----15----20
Interval:                     [15--18]
Output:    [1--------6]
                       [8-10]
                              [15--18]
                              ↑
                              |
                              +--- Add as new separate interval
```

## Final Output

```
[[1,6], [8,10], [15,18]]
```

## Edge Case Example

Let's examine a slightly different example:
`intervals = [[1,2], [3,6], [5,6]]`

Processing [1,2]:
```
Timeline: 0----5----10
Interval:  [1-2]
Output:    [1-2]
```

Processing [3,6]:
```
Timeline: 0----5----10
Interval:       [3------6]
Output:    [1-2]
                [3------6]
```

Processing [5,6]:
```
Timeline: 0----5----10
Interval:           [5-6]
Output:    [1-2]
                [3------6]
                    ↑
                    |
                    +--- Already covered, so no change
```

Final output: `[[1,2], [3,6]]`

## Key Insights

- The sorting step is crucial - it ensures we process intervals in order of start time
- We only need to compare each interval with the last merged interval in our output
- Overlapping check: if the current interval's start is less than or equal to the last end
- When merging, we take the maximum of the two end values to ensure complete coverage
- 
#### JavaScript
```javascript
function merge(intervals) {
    intervals.sort((a, b) => a[0] - b[0]);
    const output = [intervals[0]];
    
    for (let i = 1; i < intervals.length; i++) {
        const [start, end] = intervals[i];
        const lastEnd = output[output.length - 1][1];
        
        if (start <= lastEnd) {
            output[output.length - 1][1] = Math.max(lastEnd, end);
        } else {
            output.push([start, end]);
        }
    }
    
    return output;
}
```

#### Java
```java
public int[][] merge(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    
    List<int[]> output = new ArrayList<>();
    output.add(intervals[0]);
    
    for (int i = 1; i < intervals.length; i++) {
        int[] currentInterval = intervals[i];
        int[] lastInterval = output.get(output.size() - 1);
        
        if (currentInterval[0] <= lastInterval[1]) {
            lastInterval[1] = Math.max(lastInterval[1], currentInterval[1]);
        } else {
            output.add(currentInterval);
        }
    }
    
    return output.toArray(new int[output.size()][]);
}
```

### 2. Sweep Line Algorithm
- **Time complexity**: O(n log n)
- **Space complexity**: O(n)

#### Python
```python
from collections import defaultdict

def merge(intervals):
    events = []
    
    for start, end in intervals:
        events.append((start, 1))  # 1 for start event
        events.append((end, -1))   # -1 for end event
    
    events.sort()
    
    result = []
    active = 0
    start = None
    
    for time, event_type in events:
        if active == 0 and event_type == 1:
            start = time
        
        active += event_type
        
        if active == 0:
            result.append([start, time])
    
    return result
```

#### JavaScript
```javascript
function merge(intervals) {
    const events = [];
    
    for (const [start, end] of intervals) {
        events.push([start, 1]);  // 1 for start event
        events.push([end, -1]);   // -1 for end event
    }
    
    events.sort((a, b) => a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]);
    
    const result = [];
    let active = 0;
    let start = null;
    
    for (const [time, eventType] of events) {
        if (active === 0 && eventType === 1) {
            start = time;
        }
        
        active += eventType;
        
        if (active === 0) {
            result.push([start, time]);
        }
    }
    
    return result;
}
```

#### Java
```java
public int[][] merge(int[][] intervals) {
    // Create events array with start and end events
    List<int[]> events = new ArrayList<>();
    
    for (int[] interval : intervals) {
        events.add(new int[]{interval[0], 1});  // 1 for start event
        events.add(new int[]{interval[1], -1}); // -1 for end event
    }
    
    // Sort events by time, with start events before end events of same time
    Collections.sort(events, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
    
    List<int[]> result = new ArrayList<>();
    int active = 0;
    int start = 0;
    
    for (int[] event : events) {
        int time = event[0];
        int eventType = event[1];
        
        if (active == 0 && eventType == 1) {
            start = time;
        }
        
        active += eventType;
        
        if (active == 0) {
            result.add(new int[]{start, time});
        }
    }
    
    return result.toArray(new int[result.size()][]);
}
```

### 3. Greedy
- **Time complexity**: O(n log n)
- **Space complexity**: O(n)

#### Python
```python
def merge(intervals):
    if not intervals:
        return []
    
    # Sort intervals by start time
    intervals.sort(key=lambda x: x[0])
    
    merged = []
    current = intervals[0]
    
    for interval in intervals[1:]:
        # If current interval overlaps with the next one, merge them
        if current[1] >= interval[0]:
            current[1] = max(current[1], interval[1])
        else:
            # If they don't overlap, add current to result and update current
            merged.append(current)
            current = interval
    
    # Don't forget to add the last interval
    merged.append(current)
    
    return merged
```

#### JavaScript
```javascript
function merge(intervals) {
    if (!intervals.length) {
        return [];
    }
    
    // Sort intervals by start time
    intervals.sort((a, b) => a[0] - b[0]);
    
    const merged = [];
    let current = intervals[0];
    
    for (let i = 1; i < intervals.length; i++) {
        const interval = intervals[i];
        
        // If current interval overlaps with the next one, merge them
        if (current[1] >= interval[0]) {
            current[1] = Math.max(current[1], interval[1]);
        } else {
            // If they don't overlap, add current to result and update current
            merged.push(current);
            current = interval;
        }
    }
    
    // Don't forget to add the last interval
    merged.push(current);
    
    return merged;
}
```

#### Java
```java
public int[][] merge(int[][] intervals) {
    if (intervals.length <= 1) {
        return intervals;
    }
    
    // Sort intervals by start time
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    
    List<int[]> merged = new ArrayList<>();
    int[] current = intervals[0];
    
    for (int i = 1; i < intervals.length; i++) {
        int[] interval = intervals[i];
        
        // If current interval overlaps with the next one, merge them
        if (current[1] >= interval[0]) {
            current[1] = Math.max(current[1], interval[1]);
        } else {
            // If they don't overlap, add current to result and update current
            merged.add(current);
            current = interval;
        }
    }
    
    // Don't forget to add the last interval
    merged.add(current);
    
    return merged.toArray(new int[merged.size()][]);
}
```
