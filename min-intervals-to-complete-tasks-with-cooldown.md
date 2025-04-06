# Task Coordination Problem

## Problem Statement

Given a list of tasks, each represented by a letter (from A to Z), and a cooldown period `k`. The cooldown period specifies that the same task must be separated by at least `k` intervals between executions.

Determine the minimum number of intervals required to complete all tasks, while ensuring that the same task is not executed again before the cooldown period has passed.

### Input

- `tasks`: An array of characters, where each task is represented as a letter (A-Z)
- `k`: An integer representing the cooldown period, the minimum number of intervals between two executions of the same task

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

Explanation: The tasks 'A', 'B', and 'C' appear multiple times, but the cooldown is only 1. After completing one 'A', we can immediately complete another 'A' after just one other task. A valid sequence could look like this: 'A' -> 'C' -> 'A' -> 'B' -> 'D' -> 'B'. Thus, the total number of intervals required is 6.

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

The key insight to solving this problem efficiently is recognizing that the most frequent task(s) will determine the minimum number of intervals needed.

### Explanation with ASCII Diagrams

Let's work through the examples to understand the approach:

#### Example 1: tasks = ["A","A","A","B","B","B"], k = 2

First, count the frequencies:

```
A: 3 occurrences
B: 3 occurrences
```

The most frequent tasks are A and B, both occurring 3 times with max frequency = 3.

Let's visualize a possible execution sequence:

```
Position: 1  2  3  4  5  6  7  8
Tasks:    A  B  _  A  B  _  A  B
          ^     ^     ^
          |     |     |
A executions    |     |
             cooldown |
                      |
```

Every A must have at least 2 positions between them (similarly for B).
The total intervals required is 8.

#### Example 2: tasks = ["A","C","A","B","D","B"], k = 1

The frequencies are:

```
A: 2 occurrences
B: 2 occurrences
C: 1 occurrence
D: 1 occurrence
```

With cooldown k = 1, we need at least 1 interval between the same tasks:

```
Position: 1  2  3  4  5  6
Tasks:    A  C  A  B  D  B
          ^     ^
          |     |
A executions    |
             cooldown
```

We can arrange tasks without any idle intervals because the cooldown is only 1 and we have enough different tasks. The total intervals required is 6.

#### Example 3: tasks = ["A","A","A","B","B","B"], k = 3

The frequencies are the same as Example 1, but with a larger cooldown:

```
A: 3 occurrences
B: 3 occurrences
```

With k = 3, we need at least 3 positions between the same tasks:

```
Position: 1  2  3  4  5  6  7  8  9  10
Tasks:    A  B  _  _  A  B  _  _  A  B
          ^           ^           ^
          |           |           |
A executions          |           |
                   cooldown       |
                                  |
```

Here, we need to add idle intervals (\_) to satisfy the cooldown constraint. The total intervals required is 10.

### Mathematical Formula

The formula to calculate the minimum number of intervals is:

```
min_intervals = max((maxFrequency - 1) * (k + 1) + maxFrequencyCount, totalTasks)
```

Where:

- `maxFrequency` is the highest number of occurrences of any task
- `maxFrequencyCount` is the number of tasks that have this maximum frequency
- `totalTasks` is the total number of tasks to execute

#### Breaking down the formula:

1. `(maxFrequency - 1)` represents the number of gaps between occurrences of the most frequent task
2. Each gap needs to be at least of size `(k + 1)` to accommodate the cooldown
3. `maxFrequencyCount` represents the number of tasks that need to be placed at the end
4. We take the maximum of this calculation and the total number of tasks because in some cases, we might have enough different tasks to avoid idle intervals

## Implementations

### Python Solution

```python
def leastInterval(tasks, k):
    """
    Calculate the minimum number of intervals required to complete all tasks with cooldown

    Args:
        tasks: List[str] - Array of tasks represented as uppercase letters
        k: int - Cooldown period between same tasks

    Returns:
        int - Minimum number of intervals required
    """
    # Edge case: if k is 0, we can execute tasks consecutively
    if k == 0:
        return len(tasks)

    # Count the frequency of each task
    frequency_map = {}
    for task in tasks:
        frequency_map[task] = frequency_map.get(task, 0) + 1

    # Find the maximum frequency
    max_frequency = 0
    max_frequency_count = 0

    for task, frequency in frequency_map.items():
        if frequency > max_frequency:
            max_frequency = frequency
            max_frequency_count = 1
        elif frequency == max_frequency:
            max_frequency_count += 1

    # Calculate the minimum intervals needed
    min_intervals = max(
        (max_frequency - 1) * (k + 1) + max_frequency_count,
        len(tasks)
    )

    return min_intervals

# Test cases
print(leastInterval(["A","A","A","B","B","B"], 2))  # Output: 8
print(leastInterval(["A","C","A","B","D","B"], 1))  # Output: 6
print(leastInterval(["A","A","A","B","B","B"], 3))  # Output: 10
```

### JavaScript Solution

```javascript
/**
 * Calculate the minimum number of intervals required to complete all tasks with cooldown
 * @param {string[]} tasks - Array of tasks represented as uppercase letters
 * @param {number} k - Cooldown period between same tasks
 * @return {number} - Minimum number of intervals required
 */
function leastInterval(tasks, k) {
  // Edge case: if k is 0, we can execute tasks consecutively
  if (k === 0) return tasks.length;

  // Count the frequency of each task
  const frequencyMap = new Map();
  for (const task of tasks) {
    frequencyMap.set(task, (frequencyMap.get(task) || 0) + 1);
  }

  // Find the maximum frequency
  let maxFrequency = 0;
  let maxFrequencyCount = 0;

  for (const [task, frequency] of frequencyMap) {
    if (frequency > maxFrequency) {
      maxFrequency = frequency;
      maxFrequencyCount = 1;
    } else if (frequency === maxFrequency) {
      maxFrequencyCount++;
    }
  }

  // Calculate the minimum intervals needed
  const minIntervals = Math.max(
    (maxFrequency - 1) * (k + 1) + maxFrequencyCount,
    tasks.length
  );

  return minIntervals;
}

// Test cases
console.log(leastInterval(["A", "A", "A", "B", "B", "B"], 2)); // Output: 8
console.log(leastInterval(["A", "C", "A", "B", "D", "B"], 1)); // Output: 6
console.log(leastInterval(["A", "A", "A", "B", "B", "B"], 3)); // Output: 10
```

### Java Solution

```java
import java.util.HashMap;
import java.util.Map;

public class TaskCoordination {
    /**
     * Calculate the minimum number of intervals required to complete all tasks with cooldown
     *
     * @param tasks Array of tasks represented as characters
     * @param k Cooldown period between same tasks
     * @return Minimum number of intervals required
     */
    public static int leastInterval(char[] tasks, int k) {
        // Edge case: if k is 0, we can execute tasks consecutively
        if (k == 0) {
            return tasks.length;
        }

        // Count the frequency of each task
        Map<Character, Integer> frequencyMap = new HashMap<>();
        for (char task : tasks) {
            frequencyMap.put(task, frequencyMap.getOrDefault(task, 0) + 1);
        }

        // Find the maximum frequency
        int maxFrequency = 0;
        int maxFrequencyCount = 0;

        for (int frequency : frequencyMap.values()) {
            if (frequency > maxFrequency) {
                maxFrequency = frequency;
                maxFrequencyCount = 1;
            } else if (frequency == maxFrequency) {
                maxFrequencyCount++;
            }
        }

        // Calculate the minimum intervals needed
        int minIntervals = Math.max(
            (maxFrequency - 1) * (k + 1) + maxFrequencyCount,
            tasks.length
        );

        return minIntervals;
    }

    public static void main(String[] args) {
        // Test cases
        System.out.println(leastInterval(new char[]{'A','A','A','B','B','B'}, 2)); // Output: 8
        System.out.println(leastInterval(new char[]{'A','C','A','B','D','B'}, 1)); // Output: 6
        System.out.println(leastInterval(new char[]{'A','A','A','B','B','B'}, 3)); // Output: 10
    }
}
```

## Time and Space Complexity

- **Time Complexity**: O(n) where n is the number of tasks. We iterate through the tasks once to count frequencies.
- **Space Complexity**: O(1) since there are at most 26 different tasks (A-Z), so our frequency map will have at most 26 entries.
