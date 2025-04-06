# Task Coordination Problem

## Problem Statement

Given a list of tasks, each represented by a letter (from A to Z), and a cooldown period `k`. The cooldown period specifies that the same task must be separated by at least `k` intervals between executions.

Determine the minimum number of intervals required to complete all tasks, while ensuring that the same task is not executed again before the cooldown period has passed.

### Input

- `tasks: string[]`: An array of characters, where each task is represented as a letter (A-Z)
- `k: number`: An integer representing the cooldown period, the minimum number of intervals between two executions of the same task

### Examples

**Example 1:**

```
Input: tasks = ["A","A","A","B","B","B"], k = 2
Output: 8
```

Explanation: Task 'A' appears 3 times and task 'B' appears 3 times. Since there is a cooldown of 2, we need to ensure that after every 'A' there are at least 2 other tasks or idle intervals before we can add another 'A'. The same applies to 'B'. A valid sequence might look like this: 'A' -> 'B' -> idle -> 'A' -> 'B' -> idle -> 'A' -> 'B'. Thus, the total number of intervals required is 8.

**Example 2:**

```
Input: tasks = ["A","C","A","B","D","B"], k = 1
Output: 6
```

Explanation: The tasks 'A', 'B', 'C', and 'D' appear multiple times, but the cooldown is only 1. After completing one 'A', we need at least one other task before executing 'A' again. A valid sequence could look like this: 'A' -> 'C' -> 'A' -> 'B' -> 'D' -> 'B'. Thus, the total number of intervals required is 6.

**Example 3:**

```
Input: tasks = ["A","A","A","B","B","B"], k = 3
Output: 10
```

Explanation: The cooldown is 3, meaning after completing a task, we must wait for 3 intervals before repeating the same task. A valid sequence could be: 'A' -> 'B' -> idle -> idle -> 'A' -> 'B' -> idle -> idle -> 'A' -> 'B'. Thus, the total number of intervals needed is 10.

### Constraints

- 1 <= `tasks.length` <= 1000
- `tasks[i]` is an uppercase English letter
- 0 <= `k` <= 100

## Solution Approach

The provided solution follows a different approach compared to the standard formula. Let's break it down step by step:

1. Count the frequency of each task (how many times each task appears)
2. Find the maximum frequency and count how many tasks have this maximum frequency
3. Calculate the idle slots needed based on these values
4. Determine the final result

### Key Variables in the Solution

- `maximum`: The highest frequency of any task
- `maxCount`: The number of tasks that have this maximum frequency
- `partCount`: Number of gaps between occurrences of the most frequent tasks (maximum - 1)
- `partLength`: Maximum number of other tasks that can fit in each gap (k - (maxCount - 1))
- `emptySlots`: Total number of potential empty slots (partCount \* partLength)
- `availableTasks`: Number of tasks available to fill empty slots (tasks.length - maximum \* maxCount)
- `idles`: Number of slots that must remain idle (max(0, emptySlots - availableTasks))

### Explanation with ASCII Diagrams

To better understand this solution, let's visualize the process with ASCII diagrams using our examples.

#### Example 1: tasks = ["A","A","A","B","B","B"], k = 2

First, count the frequencies:

```
A: 3 occurrences
B: 3 occurrences
```

Maximum frequency = 3, and maxCount = 2 (both A and B have max frequency)

We can arrange tasks to minimize idle time by scheduling the most frequent tasks first, then filling in with less frequent tasks:

```
| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| A | B |   | A | B |   | A | B |
```

In our solution:

- `partCount` = maximum - 1 = 3 - 1 = 2 (two gaps between three A's)
- `partLength` = k - (maxCount - 1) = 2 - (2 - 1) = 1 (one slot in each gap)
- `emptySlots` = partCount _ partLength = 2 _ 1 = 2
- `availableTasks` = tasks.length - maximum _ maxCount = 6 - 3 _ 2 = 0
- `idles` = max(0, emptySlots - availableTasks) = max(0, 2 - 0) = 2
- Total intervals = tasks.length + idles = 6 + 2 = 8

#### Example 2: tasks = ["A","C","A","B","D","B"], k = 1

The frequencies are:

```
A: 2 occurrences
B: 2 occurrences
C: 1 occurrence
D: 1 occurrence
```

Maximum frequency = 2, and maxCount = 2 (both A and B have max frequency)

When arranged optimally:

```
| 1 | 2 | 3 | 4 | 5 | 6 |
| A | C | A | B | D | B |
```

In our solution:

- `partCount` = maximum - 1 = 2 - 1 = 1 (one gap between two A's)
- `partLength` = k - (maxCount - 1) = 1 - (2 - 1) = 0 (no slots in each gap)
- `emptySlots` = partCount _ partLength = 1 _ 0 = 0
- `availableTasks` = tasks.length - maximum _ maxCount = 6 - 2 _ 2 = 2
- `idles` = max(0, emptySlots - availableTasks) = max(0, 0 - 2) = 0
- Total intervals = tasks.length + idles = 6 + 0 = 6

#### Example 3: tasks = ["A","A","A","B","B","B"], k = 3

With the same frequencies as Example 1 but a larger cooldown:

```
| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| A | B |   |   | A | B |   |   | A | B  |
```

In our solution:

- `partCount` = maximum - 1 = 3 - 1 = 2 (two gaps between three A's)
- `partLength` = k - (maxCount - 1) = 3 - (2 - 1) = 2 (two slots in each gap)
- `emptySlots` = partCount _ partLength = 2 _ 2 = 4
- `availableTasks` = tasks.length - maximum _ maxCount = 6 - 3 _ 2 = 0
- `idles` = max(0, emptySlots - availableTasks) = max(0, 4 - 0) = 4
- Total intervals = tasks.length + idles = 6 + 4 = 10

## Implementations

### JavaScript Implementation

```javascript
/**
 * Calculate the minimum number of intervals required to complete all tasks with cooldown
 * @param {string[]} tasks - Array of tasks represented as uppercase letters
 * @param {number} k - Cooldown period between same tasks
 * @return {number} - Minimum number of intervals required
 */
function taskCoordinator(tasks, k) {
  // Array to store the frequency of each task (26 letters, A-Z)
  const counter = new Array(26).fill(0);
  let maximum = 0; // Maximum frequency of any task
  let maxCount = 0; // Number of tasks with maximum frequency

  // Traverse through tasks to calculate task frequencies
  for (const task of tasks) {
    const index = task.charCodeAt(0) - "A".charCodeAt(0);
    counter[index]++;

    if (maximum === counter[index]) {
      maxCount++;
    } else if (maximum < counter[index]) {
      maximum = counter[index];
      maxCount = 1;
    }
  }

  // Calculate idle slots, available tasks, and idles needed
  const partCount = maximum - 1;
  const partLength = k - (maxCount - 1);
  const emptySlots = partCount * partLength;
  const availableTasks = tasks.length - maximum * maxCount;
  const idles = Math.max(0, emptySlots - availableTasks);

  // Return the total time required
  return tasks.length + idles;
}
```

### Python Implementation

```python
def taskCoordinator(tasks, k):
    """
    Calculate the minimum number of intervals required to complete all tasks with cooldown

    Args:
        tasks: List[str] - Array of tasks represented as uppercase letters
        k: int - Cooldown period between same tasks

    Returns:
        int - Minimum number of intervals required
    """
    # Array to store the frequency of each task (26 letters, A-Z)
    counter = [0] * 26
    maximum = 0  # Maximum frequency of any task
    max_count = 0  # Number of tasks with maximum frequency

    # Traverse through tasks to calculate task frequencies
    for task in tasks:
        index = ord(task) - ord('A')
        counter[index] += 1

        if maximum == counter[index]:
            max_count += 1
        elif maximum < counter[index]:
            maximum = counter[index]
            max_count = 1

    # Calculate idle slots, available tasks, and idles needed
    part_count = maximum - 1
    part_length = k - (max_count - 1)
    empty_slots = part_count * part_length
    available_tasks = len(tasks) - maximum * max_count
    idles = max(0, empty_slots - available_tasks)

    # Return the total time required
    return len(tasks) + idles
```

### Java Implementation

```java
public class TaskCoordinator {
    /**
     * Calculate the minimum number of intervals required to complete all tasks with cooldown
     *
     * @param tasks Array of tasks represented as characters
     * @param k Cooldown period between same tasks
     * @return Minimum number of intervals required
     */
    public static int taskCoordinator(char[] tasks, int k) {
        // Array to store the frequency of each task (26 letters, A-Z)
        int[] counter = new int[26];
        int maximum = 0; // Maximum frequency of any task
        int maxCount = 0; // Number of tasks with maximum frequency

        // Traverse through tasks to calculate task frequencies
        for (char task : tasks) {
            int index = task - 'A';
            counter[index]++;

            if (maximum == counter[index]) {
                maxCount++;
            } else if (maximum < counter[index]) {
                maximum = counter[index];
                maxCount = 1;
            }
        }

        // Calculate idle slots, available tasks, and idles needed
        int partCount = maximum - 1;
        int partLength = k - (maxCount - 1);
        int emptySlots = partCount * partLength;
        int availableTasks = tasks.length - maximum * maxCount;
        int idles = Math.max(0, emptySlots - availableTasks);

        // Return the total time required
        return tasks.length + idles;
    }

    public static void main(String[] args) {
        // Test cases
        System.out.println(taskCoordinator(new char[]{'A','A','A','B','B','B'}, 2)); // Output: 8
        System.out.println(taskCoordinator(new char[]{'A','C','A','B','D','B'}, 1)); // Output: 6
        System.out.println(taskCoordinator(new char[]{'A','A','A','B','B','B'}, 3)); // Output: 10
    }
}
```

## Time and Space Complexity

- **Time Complexity**: O(n) where n is the number of tasks. We iterate through the tasks once to count frequencies.
- **Space Complexity**: O(1) since we use a fixed-size array of 26 elements to store the frequencies of tasks (A-Z).
