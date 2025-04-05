# Meeting Rooms II

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` (si < ei), find the minimum number of conference rooms required.

**Note**: The format `(0,8),(8,10)` is not considered a conflict at 8.

## Approaches

### 1. Min Heap
- **Time complexity**: O(n log n)
- **Space complexity**: O(n)

#### Python
```python
import heapq

def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Sort intervals by start time
    intervals.sort(key=lambda x: x[0])
    
    # Use a min heap to track the minimum end time of allocated rooms
    rooms = []
    heapq.heappush(rooms, intervals[0][1])
    
    for i in range(1, len(intervals)):
        # If the current meeting starts after the earliest ending meeting,
        # we can reuse the same room
        if intervals[i][0] >= rooms[0]:
            heapq.heappop(rooms)
        
        # Allocate a room for the current meeting
        heapq.heappush(rooms, intervals[i][1])
    
    # The size of the heap is the minimum number of rooms required
    return len(rooms)
```

#### JavaScript
```javascript
function minMeetingRooms(intervals) {
    if (!intervals.length) {
        return 0;
    }
    
    // Sort intervals by start time
    intervals.sort((a, b) => a[0] - b[0]);
    
    // Use a min heap to track the minimum end time of allocated rooms
    const rooms = [intervals[0][1]];
    
    for (let i = 1; i < intervals.length; i++) {
        // If the current meeting starts after the earliest ending meeting,
        // we can reuse the same room
        if (intervals[i][0] >= rooms[0]) {
            rooms.shift();
        }
        
        // Allocate a room for the current meeting
        insertMinHeap(rooms, intervals[i][1]);
    }
    
    // The size of the heap is the minimum number of rooms required
    return rooms.length;
    
    // Helper function to insert into min heap
    function insertMinHeap(heap, val) {
        heap.push(val);
        let idx = heap.length - 1;
        
        while (idx > 0) {
            const parentIdx = Math.floor((idx - 1) / 2);
            if (heap[parentIdx] <= heap[idx]) break;
            
            [heap[parentIdx], heap[idx]] = [heap[idx], heap[parentIdx]];
            idx = parentIdx;
        }
        
        heap.sort((a, b) => a - b);  // Simplification for JavaScript
    }
}
```

#### Java
```java
public int minMeetingRooms(int[][] intervals) {
    if (intervals == null || intervals.length == 0) {
        return 0;
    }
    
    // Sort intervals by start time
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    
    // Use a min heap to track the minimum end time of allocated rooms
    PriorityQueue<Integer> rooms = new PriorityQueue<>();
    rooms.add(intervals[0][1]);
    
    for (int i = 1; i < intervals.length; i++) {
        // If the current meeting starts after the earliest ending meeting,
        // we can reuse the same room
        if (intervals[i][0] >= rooms.peek()) {
            rooms.poll();
        }
        
        // Allocate a room for the current meeting
        rooms.add(intervals[i][1]);
    }
    
    // The size of the heap is the minimum number of rooms required
    return rooms.size();
}
```

### 2. Sweep Line Algorithm
- **Time complexity**: O(n log n)
- **Space complexity**: O(n)

#### Python
```python
from collections import defaultdict

def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Create events array with start and end events
    events = []
    for start, end in intervals:
        events.append((start, 1))   # 1 for start event
        events.append((end, -1))    # -1 for end event
    
    # Sort events by time, with start events before end events of same time
    events.sort()
    
    current_rooms = 0
    max_rooms = 0
    
    for time, event_type in events:
        current_rooms += event_type
        max_rooms = max(max_rooms, current_rooms)
    
    return max_rooms
```

#### JavaScript
```javascript
function minMeetingRooms(intervals) {
    if (!intervals.length) {
        return 0;
    }
    
    // Create events array with start and end events
    const events = [];
    for (const [start, end] of intervals) {
        events.push([start, 1]);    // 1 for start event
        events.push([end, -1]);     // -1 for end event
    }
    
    // Sort events by time, with start events before end events of same time
    events.sort((a, b) => a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]);
    
    let currentRooms = 0;
    let maxRooms = 0;
    
    for (const [time, eventType] of events) {
        currentRooms += eventType;
        maxRooms = Math.max(maxRooms, currentRooms);
    }
    
    return maxRooms;
}
```

#### Java
```java
public int minMeetingRooms(int[][] intervals) {
    if (intervals == null || intervals.length == 0) {
        return 0;
    }
    
    // Create events array with start and end events
    List<int[]> events = new ArrayList<>();
    for (int[] interval : intervals) {
        events.add(new int[]{interval[0], 1});   // 1 for start event
        events.add(new int[]{interval[1], -1});  // -1 for end event
    }
    
    // Sort events by time, with start events before end events of same time
    Collections.sort(events, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
    
    int currentRooms = 0;
    int maxRooms = 0;
    
    for (int[] event : events) {
        currentRooms += event[1];
        maxRooms = Math.max(maxRooms, currentRooms);
    }
    
    return maxRooms;
}
```

### 3. Two Pointers
- **Time complexity**: O(n log n)
- **Space complexity**: O(n)

#### Python
```python
def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Separate start and end times
    start_times = sorted([interval[0] for interval in intervals])
    end_times = sorted([interval[1] for interval in intervals])
    
    # Initialize pointers
    s = e = 0
    current_rooms = max_rooms = 0
    
    # Process all start and end times in order
    while s < len(intervals):
        if start_times[s] < end_times[e]:
            # A new meeting starts, we need a new room
            current_rooms += 1
            s += 1
        else:
            # A meeting ends, we can reuse a room
            current_rooms -= 1
            e += 1
        
        max_rooms = max(max_rooms, current_rooms)
    
    return max_rooms
```


## Algorithm Overview

1. Extract and sort all meeting start times
2. Extract and sort all meeting end times
3. Use two pointers to iterate through these sorted arrays
4. When we encounter a start time, we need a new meeting room (count++)
5. When we encounter an end time, we free up a meeting room (count--)
6. Track the maximum count encountered, which represents the minimum rooms needed

## Visual Explanation with ASCII Diagrams

Let's work through an example:
```
intervals = [(1,4), (2,5), (7,9)]
```

### Step 1: Sort start and end times separately
```
start = [1, 2, 7]
end = [4, 5, 9]
```

### Step 2: Initialize variables
```
res = 0   (maximum rooms needed)
count = 0 (current rooms in use)
s = 0     (pointer for start array)
e = 0     (pointer for end array)
```

### Step 3: Iterate through the arrays with two pointers

#### Iteration 1:
Compare start[0] = 1 with end[0] = 4
Since 1 < 4, a new meeting starts before any ends:
- Increment s (s = 1)
- Increment count (count = 1)
- Update res = max(res, count) = 1

```
Count: 1
Max: 1
```

#### Iteration 2:
Compare start[1] = 2 with end[0] = 4
Since 2 < 4, another meeting starts before any ends:
- Increment s (s = 2)
- Increment count (count = 2)
- Update res = max(res, count) = 2

```
Count: 2
Max: 2
```

#### Iteration 3:
Compare start[2] = 7 with end[0] = 4
Since 7 > 4, a meeting ends before the next one starts:
- Increment e (e = 1)
- Decrement count (count = 1)
- res remains 2

```
Count: 1
Max: 2
```

#### Iteration 4:
Compare start[2] = 7 with end[1] = 5
Since 7 > 5, another meeting ends before the next one starts:
- Increment e (e = 2)
- Decrement count (count = 0)
- res remains 2

```

Count: 0
Max: 2
```

#### Iteration 5:
Compare start[2] = 7 with end[2] = 9
Since 7 < 9, a new meeting starts:
- Increment s (s = 3)
- Increment count (count = 1)
- res remains 2


Since s = 3 = len(intervals), we exit the loop.

### Final result:
```
res = 2
```

## Another Example with More Overlap

Let's try a more complex example:
```
intervals = [(1,10), (2,7), (3,19), (8,12), (10,20), (11,30)]
```

Sorting the start and end times:
```
start = [1, 2, 3, 8, 10, 11]
end = [7, 10, 12, 19, 20, 30]
```

Stepping through:
1. start[0]=1 < end[0]=7: count=1, max=1
2. start[1]=2 < end[0]=7: count=2, max=2
3. start[2]=3 < end[0]=7: count=3, max=3
4. start[3]=8 > end[0]=7: count=2, max=3
5. start[3]=8 < end[1]=10: count=3, max=3
6. start[4]=10 = end[1]=10: In this algorithm, we prioritize start times, so count=4, max=4
7. start[5]=11 > end[1]=10: count=3, max=4
8. start[5]=11 < end[2]=12: count=4, max=4
9. (s=6) exceeds array length, algorithm terminates

Result: We need 4 meeting rooms to accommodate all meetings.

## Key Insights

1. The two-pointer approach elegantly tracks room usage over time

2. By separating and sorting start/end times, we create a timeline of events

3. When a meeting starts, we need one more room; when it ends, we free one room

4. The maximum number of simultaneous meetings represents the minimum rooms needed

5. This algorithm has O(n log n) time complexity due to the sorting operations

6. In case of ties (same start and end time), this implementation prioritizes start times, which is necessary for correct results
   

#### JavaScript
```javascript
function minMeetingRooms(intervals) {
    if (!intervals.length) {
        return 0;
    }
    
    // Separate start and end times
    const startTimes = intervals.map(interval => interval[0]).sort((a, b) => a - b);
    const endTimes = intervals.map(interval => interval[1]).sort((a, b) => a - b);
    
    // Initialize pointers
    let s = 0, e = 0;
    let currentRooms = 0, maxRooms = 0;
    
    // Process all start and end times in order
    while (s < intervals.length) {
        if (startTimes[s] < endTimes[e]) {
            // A new meeting starts, we need a new room
            currentRooms++;
            s++;
        } else {
            // A meeting ends, we can reuse a room
            currentRooms--;
            e++;
        }
        
        maxRooms = Math.max(maxRooms, currentRooms);
    }
    
    return maxRooms;
}
```

#### Java
```java
public int minMeetingRooms(int[][] intervals) {
    if (intervals == null || intervals.length == 0) {
        return 0;
    }
    
    // Separate start and end times
    int[] startTimes = new int[intervals.length];
    int[] endTimes = new int[intervals.length];
    
    for (int i = 0; i < intervals.length; i++) {
        startTimes[i] = intervals[i][0];
        endTimes[i] = intervals[i][1];
    }
    
    Arrays.sort(startTimes);
    Arrays.sort(endTimes);
    
    // Initialize pointers
    int s = 0, e = 0;
    int currentRooms = 0, maxRooms = 0;
    
    // Process all start and end times in order
    while (s < intervals.length) {
        if (startTimes[s] < endTimes[e]) {
            // A new meeting starts, we need a new room
            currentRooms++;
            s++;
        } else {
            // A meeting ends, we can reuse a room
            currentRooms--;
            e++;
        }
        
        maxRooms = Math.max(maxRooms, currentRooms);
    }
    
    return maxRooms;
}
```

### 4. Greedy Approach
- **Time complexity**: O(n log n)
- **Space complexity**: O(n)

#### Python
```python
def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Create time events with labels
    time_events = []
    for start, end in intervals:
        time_events.append((start, 1))  # 1 for start time
        time_events.append((end, -1))   # -1 for end time
    
    # Sort events by time, then by type (ends before starts)
    time_events.sort(key=lambda x: (x[0], x[1]))
    
    current_rooms = 0
    max_rooms = 0
    
    for time, event_type in time_events:
        current_rooms += event_type
        max_rooms = max(max_rooms, current_rooms)
    
    return max_rooms
```

#### JavaScript
```javascript
function minMeetingRooms(intervals) {
    if (!intervals.length) {
        return 0;
    }
    
    // Create time events with labels
    const timeEvents = [];
    for (const [start, end] of intervals) {
        timeEvents.push([start, 1]);   // 1 for start time
        timeEvents.push([end, -1]);    // -1 for end time
    }
    
    // Sort events by time, then by type (ends before starts)
    timeEvents.sort((a, b) => a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]);
    
    let currentRooms = 0;
    let maxRooms = 0;
    
    for (const [time, eventType] of timeEvents) {
        currentRooms += eventType;
        maxRooms = Math.max(maxRooms, currentRooms);
    }
    
    return maxRooms;
}
```

#### Java
```java
public int minMeetingRooms(int[][] intervals) {
    if (intervals == null || intervals.length == 0) {
        return 0;
    }
    
    // Create time events with labels
    List<int[]> timeEvents = new ArrayList<>();
    for (int[] interval : intervals) {
        timeEvents.add(new int[]{interval[0], 1});   // 1 for start time
        timeEvents.add(new int[]{interval[1], -1});  // -1 for end time
    }
    
    // Sort events by time, then by type (ends before starts)
    Collections.sort(timeEvents, (a, b) -> 
        a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
    
    int currentRooms = 0;
    int maxRooms = 0;
    
    for (int[] event : timeEvents) {
        currentRooms += event[1];
        maxRooms = Math.max(maxRooms, currentRooms);
    }
    
    return maxRooms;
}
```
