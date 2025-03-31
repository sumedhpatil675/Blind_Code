# Meeting Rooms

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` (si < ei), determine if a person could attend all meetings.

**Note**: The format `(0,8),(8,10)` is not considered a conflict at 8.

## Approaches

### 1. Brute Force
- **Time complexity**: O(n²)
- **Space complexity**: O(1)

#### Python
```python
def canAttendMeetings(intervals):
    n = len(intervals)
    
    for i in range(n):
        A = intervals[i]
        for j in range(i + 1, n):
            B = intervals[j]
            if min(A[1], B[1]) > max(A[0], B[0]):  # Check for overlap
                return False
    
    return True
```

#### JavaScript
```javascript
function canAttendMeetings(intervals) {
    const n = intervals.length;
    
    for (let i = 0; i < n; i++) {
        const A = intervals[i];
        for (let j = i + 1; j < n; j++) {
            const B = intervals[j];
            if (Math.min(A[1], B[1]) > Math.max(A[0], B[0])) {  // Check for overlap
                return false;
            }
        }
    }
    
    return true;
}
```

#### Java
```java
public boolean canAttendMeetings(int[][] intervals) {
    int n = intervals.length;
    
    for (int i = 0; i < n; i++) {
        int[] A = intervals[i];
        for (int j = i + 1; j < n; j++) {
            int[] B = intervals[j];
            if (Math.min(A[1], B[1]) > Math.max(A[0], B[0])) {  // Check for overlap
                return false;
            }
        }
    }
    
    return true;
}
```

### 2. Sorting
- **Time complexity**: O(n log n)
- **Space complexity**: O(1) or O(n) depending on the sorting algorithm

#### Python
```python
def canAttendMeetings(intervals):
    intervals.sort(key=lambda i: i[0])  # Sort by start time
    
    for i in range(1, len(intervals)):
        if intervals[i-1][1] > intervals[i][0]:
            return False
    
    return True
```

#### JavaScript
```javascript
function canAttendMeetings(intervals) {
    intervals.sort((a, b) => a[0] - b[0]);  // Sort by start time
    
    for (let i = 1; i < intervals.length; i++) {
        if (intervals[i-1][1] > intervals[i][0]) {
            return false;
        }
    }
    
    return true;
}
```

#### Java
```java
public boolean canAttendMeetings(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));  // Sort by start time
    
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i-1][1] > intervals[i][0]) {
            return false;
        }
    }
    
    return true;
}
```

### 3. Sweep Line
- **Time complexity**: O(n log n)
- **Space complexity**: O(n)

#### Python
```python
def canAttendMeetings(intervals):
    events = []
    
    for start, end in intervals:
        events.append((start, 1))  # 1 for start event
        events.append((end, -1))   # -1 for end event
    
    events.sort()
    
    count = 0
    for time, event_type in events:
        count += event_type
        if count > 1:  # If more than one meeting at the same time
            return False
    
    return True
```

#### JavaScript
```javascript
function canAttendMeetings(intervals) {
    const events = [];
    
    for (const [start, end] of intervals) {
        events.push([start, 1]);   // 1 for start event
        events.push([end, -1]);    // -1 for end event
    }
    
    events.sort((a, b) => a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]);
    
    let count = 0;
    for (const [time, eventType] of events) {
        count += eventType;
        if (count > 1) {  // If more than one meeting at the same time
            return false;
        }
    }
    
    return true;
}
```

#### Java
```java
public boolean canAttendMeetings(int[][] intervals) {
    List<int[]> events = new ArrayList<>();
    
    for (int[] interval : intervals) {
        events.add(new int[]{interval[0], 1});   // 1 for start event
        events.add(new int[]{interval[1], -1});  // -1 for end event
    }
    
    Collections.sort(events, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
    
    int count = 0;
    for (int[] event : events) {
        count += event[1];
        if (count > 1) {  // If more than one meeting at the same time
            return false;
        }
    }
    
    return true;
}
```