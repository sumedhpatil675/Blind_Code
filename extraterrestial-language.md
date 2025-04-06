# Extraterrestrial Language

## Problem Statement

An extraterrestrial language uses the English alphabet, but the letter order is unknown. A list of strings `words` is provided, which represents words from the extraterrestrial language's dictionary, and it is claimed that these words are sorted according to the new language's **lexicographical order**.

If this claim is false, and the arrangement of strings in `words` cannot match any possible letter order, return an empty string. Otherwise, return a string containing the unique letters of the extraterrestrial language sorted according to the language's rules. If multiple valid solutions exist, any of them are acceptable.

### Input

- `words: string[]`: An array of strings where each string is a word in the extraterrestrial language

### Examples

**Example 1:**

```
Input: words = ["cab", "abc", "bca"]
Output: "cab"
Explanation: Comparing 'cab' and 'abc', the first differing characters are c and a, suggesting c comes before a. Comparing 'abc' and 'bca', the first differing characters are a and b, so a comes before b. Thus, the order of letters in the new alphabet is 'cab'.
```

**Example 2:**

```
Input: words = ["abc", "ab"]
Output: ""
Explanation: The list is invalid because 'ab' should not come before 'abc'. A shorter word with the same prefix cannot appear after a longer word, so the sequence is impossible, and the result is an empty string.
```

**Example 3:**

```
Input: words = ["z", "x", "z"]
Output: ""
Explanation: The sequence is invalid because 'z' appears both before and after 'x', which creates a contradiction in the order. This inconsistency makes it impossible to deduce a valid letter order, so the result is an empty string.
```

### Constraints

- 1 <= `words.length` <= 100
- 1 <= `words[i].length` <= 10
- `words[i]` consists of only lowercase English letters

## Solution Approach

This problem is fundamentally about determining if a valid topological ordering exists for a directed graph constructed from the given words. The steps to solve this are:

1. Create a directed graph where each node is a letter, and edges represent the relationships between letters (e.g., 'a' comes before 'b').
2. Extract these relationships by comparing adjacent words in the sorted list.
3. Detect if there are any invalid conditions (e.g., a shorter word with the same prefix appears after a longer word).
4. Perform a topological sort on the graph to determine the letter ordering.
5. If a valid topological sort isn't possible (i.e., the graph has a cycle), return an empty string.

## Solution Code

### Python Solution

```python
from collections import defaultdict, deque

def extraterrestrial_language(words):
    # Step 0: Create data structures and find all unique letters
    graph = defaultdict(list)
    in_degree = defaultdict(int)
    all_chars = set()

    for word in words:
        for char in word:
            all_chars.add(char)

    # Step 1: Build the graph from the word relationships
    for i in range(len(words) - 1):
        word1, word2 = words[i], words[i+1]

        # Check if word2 is a prefix of word1, which would be invalid
        if len(word1) > len(word2) and word1.startswith(word2):
            return ""

        # Find first non-matching character to establish order
        min_len = min(len(word1), len(word2))
        for j in range(min_len):
            if word1[j] != word2[j]:
                graph[word1[j]].append(word2[j])
                in_degree[word2[j]] += 1
                break

    # Step 2: Topological Sort using BFS
    queue = deque([c for c in all_chars if in_degree[c] == 0])
    result = []

    while queue:
        char = queue.popleft()
        result.append(char)

        for neighbor in graph[char]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    # If all characters are not included, there's a cycle
    if len(result) != len(all_chars):
        return ""

    return "".join(result)
```

### JavaScript Solution

```javascript
function extraterrestrialLanguage(words) {
  // Step 0: Create data structures and find all unique letters
  const adjList = {};
  const counts = {};

  // Initialize adjacency list and counts map
  for (const word of words) {
    for (const c of word) {
      if (!counts[c]) {
        counts[c] = 0;
        adjList[c] = [];
      }
    }
  }

  // Step 1: Find all edges
  for (let i = 0; i < words.length - 1; i++) {
    const word1 = words[i];
    const word2 = words[i + 1];

    // Check that word2 is not a prefix of word1
    if (word1.length > word2.length && word1.startsWith(word2)) {
      return "";
    }

    // Find the first non-match and insert the corresponding relation
    for (let j = 0; j < Math.min(word1.length, word2.length); j++) {
      if (word1[j] !== word2[j]) {
        adjList[word1[j]].push(word2[j]);
        counts[word2[j]]++;
        break;
      }
    }
  }

  // Step 2: Breadth-first search for topological sort
  const result = [];
  const queue = [];

  // Enqueue characters with no incoming edges
  for (const c in counts) {
    if (counts[c] === 0) {
      queue.push(c);
    }
  }

  // Perform BFS to build the result string
  while (queue.length > 0) {
    const c = queue.shift();
    result.push(c);

    for (const next of adjList[c]) {
      counts[next]--;
      if (counts[next] === 0) {
        queue.push(next);
      }
    }
  }

  // If the result length is less than the number of unique characters, return an empty string
  if (result.length < Object.keys(counts).length) {
    return "";
  }

  return result.join("");
}
```

### Java Solution

```java
import java.util.*;

public class ExtraterrestrialLanguage {
    public String extraterrestrialLanguage(String[] words) {
        // Step 0: Create data structures and find all unique letters
        Map<Character, List<Character>> graph = new HashMap<>();
        Map<Character, Integer> inDegree = new HashMap<>();
        Set<Character> allChars = new HashSet<>();

        // Initialize the graph and in-degree map
        for (String word : words) {
            for (char c : word.toCharArray()) {
                allChars.add(c);
                if (!graph.containsKey(c)) {
                    graph.put(c, new ArrayList<>());
                    inDegree.put(c, 0);
                }
            }
        }

        // Step 1: Build the graph
        for (int i = 0; i < words.length - 1; i++) {
            String word1 = words[i];
            String word2 = words[i + 1];

            // Check if word2 is a prefix of word1, which is invalid
            if (word1.length() > word2.length() && word1.startsWith(word2)) {
                return "";
            }

            // Find the first non-match
            int minLength = Math.min(word1.length(), word2.length());
            for (int j = 0; j < minLength; j++) {
                if (word1.charAt(j) != word2.charAt(j)) {
                    char from = word1.charAt(j);
                    char to = word2.charAt(j);
                    graph.get(from).add(to);
                    inDegree.put(to, inDegree.get(to) + 1);
                    break;
                }
            }
        }

        // Step 2: Topological Sort (BFS)
        StringBuilder result = new StringBuilder();
        Queue<Character> queue = new LinkedList<>();

        // Start with nodes that have no incoming edges
        for (char c : allChars) {
            if (inDegree.get(c) == 0) {
                queue.add(c);
            }
        }

        while (!queue.isEmpty()) {
            char current = queue.poll();
            result.append(current);

            for (char neighbor : graph.get(current)) {
                inDegree.put(neighbor, inDegree.get(neighbor) - 1);
                if (inDegree.get(neighbor) == 0) {
                    queue.add(neighbor);
                }
            }
        }

        // Check if all characters are included
        if (result.length() != allChars.size()) {
            return "";
        }

        return result.toString();
    }
}
```

## Explanation with ASCII Diagrams

Let's walk through the solution for the example `["cab", "abc", "bca"]` step by step.

### 1. Building the Directed Graph

We compare adjacent words to extract character relationships:

1. Comparing "cab" and "abc":

   - First differing characters: 'c' in "cab" and 'a' in "abc"
   - We know that 'c' comes before 'a'

2. Comparing "abc" and "bca":
   - First differing characters: 'a' in "abc" and 'b' in "bca"
   - We know that 'a' comes before 'b'

Our directed graph now looks like:

```
    c
    |
    v
    a
    |
    v
    b
```

ASCII Graph:

```
c ----> a ----> b
```

### 2. Topological Sort

Now we perform a topological sort:

1. Start with nodes with no incoming edges (in-degree = 0)
   - 'c' has in-degree 0, so we start with 'c'
2. Remove 'c' and decrement the in-degree of its neighbors
   - 'a' now has in-degree 0
3. Remove 'a' and decrement the in-degree of its neighbors
   - 'b' now has in-degree 0
4. Remove 'b'

The resulting topological order is "cab", which matches our expected output.

### Invalid Case: ["abc", "ab"]

Let's check the case `["abc", "ab"]`:

1. When we compare "abc" and "ab", we notice that "ab" is a prefix of "abc"
2. In lexicographical ordering, prefixes must come before their extensions
3. Therefore, "ab" should come before "abc", not after
4. This violates our rules, so we return an empty string

### Invalid Case: ["z", "x", "z"]

For `["z", "x", "z"]`:

1. From the first pair "z" and "x", we establish 'z' comes before 'x'
2. From the second pair "x" and "z", we establish 'x' comes before 'z'
3. This creates a cycle: z → x → z

```
z <----> x
```

Since a valid alphabet ordering must be a linear arrangement without cycles, this arrangement is impossible, and we return an empty string.

## Time and Space Complexity

- **Time Complexity**: O(n \* m), where n is the number of words and m is the maximum length of any word. We need to compare adjacent words once and then perform a topological sort.
- **Space Complexity**: O(k), where k is the number of unique characters. In the worst case, this would be the size of the alphabet (26 for lowercase English letters).

## Summary

The key insights for this problem are:

1. It's a graph problem where we need to find a valid topological ordering.
2. We need to check for two invalid conditions:
   - If a shorter word is a prefix of a longer word that appears before it.
   - If there's a cycle in the graph, indicating contradicting ordering requirements.
3. Topological sort gives us the valid character ordering if one exists.
