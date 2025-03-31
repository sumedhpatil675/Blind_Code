# Blind 75 Coding Problems - Index

This index contains links to all 75 Leetcode problems commonly known as the "Blind 75". Each problem includes solutions in Python, JavaScript, and Java, along with a brief description of the approach.

## Arrays & Hashing

| Problem | Approach |
|---------|----------|
| [Contains Duplicate](contains-duplicate.md) | Use a hash set to track seen values; return true when a duplicate is found |
| [Valid Anagram](valid-anagram.md) | Sort both strings and compare, or use a hash map to count character frequencies |
| [Two Sum](two-sum.md) | Use a hash map to store complements of previously seen values |
| [Group Anagrams](group-anagrams.md) | Create a key for each word based on sorted characters or character counts |
| [Top K Frequent Elements](top-k-frequent-elements.md) | Count frequencies and use a heap or bucket sort to find top K elements |
| [Encode and Decode Strings](encode-decode-strings.md) | Use length encoding to distinguish between strings |
| [Product of Array Except Self](product-except-self.md) | Use prefix and postfix products to avoid division |
| [Longest Consecutive Sequence](longest-consecutive-sequence.md) | Use a hash set and check for the start of sequences |

## Two Pointers

| Problem | Approach |
|---------|----------|
| [Valid Palindrome](valid-palindrome.md) | Use two pointers from the ends working inward |
| [Three Sum](three-sum.md) | Sort and use a two-pointer approach for each fixed first element |
| [Container With Most Water](container-with-most-water.md) | Two pointers from both ends, move the smaller height inward |

## Sliding Window

| Problem | Approach |
|---------|----------|
| [Best Time to Buy and Sell Stock](best-time-to-buy-sell-stock.md) | Sliding window to keep track of minimum price seen so far |
| [Longest Substring Without Repeating Characters](longest-substring-without-repeating.md) | Sliding window with a set to track characters in current window |
| [Longest Repeating Character Replacement](longest-repeating-character-replacement.md) | Sliding window tracking the most frequent character |
| [Minimum Window Substring](minimum-window-substring.md) | Sliding window with two hash maps to track required and found characters |

## Stack

| Problem | Approach |
|---------|----------|
| [Valid Parentheses](valid-parentheses.md) | Use a stack to match opening and closing brackets |

## Binary Search

| Problem | Approach |
|---------|----------|
| [Find Minimum in Rotated Sorted Array](find-minimum-rotated-sorted-array.md) | Binary search to find the pivot/rotation point |
| [Search in Rotated Sorted Array](search-rotated-sorted-array.md) | Modified binary search checking which half is sorted |

## Linked List

| Problem | Approach |
|---------|----------|
| [Reverse Linked List](reverse-linked-list.md) | Iterative or recursive approach to reverse pointers |
| [Merge Two Sorted Lists](merge-two-sorted-lists.md) | Iterate through both lists, comparing values and building a new list |
| [Reorder List](reorder-list.md) | Find middle, reverse second half, then merge two halves |
| [Remove Nth Node From End of List](remove-nth-node-from-end.md) | Two pointers, with first pointer n steps ahead |
| [Linked List Cycle](linked-list-cycle.md) | Slow and fast pointers to detect cycle |
| [Merge K Sorted Lists](merge-k-sorted-lists.md) | Use a min heap or divide and conquer approach |

## Trees

| Problem | Approach |
|---------|----------|
| [Invert Binary Tree](invert-binary-tree.md) | Swap left and right children recursively |
| [Maximum Depth of Binary Tree](maximum-depth-binary-tree.md) | Recursive DFS or iterative BFS |
| [Same Tree](same-tree.md) | Recursive comparison of nodes |
| [Subtree of Another Tree](subtree-of-another-tree.md) | Check if current node or any subtree matches |
| [Lowest Common Ancestor of a Binary Search Tree](lowest-common-ancestor-bst.md) | Navigate based on BST properties |
| [Binary Tree Level Order Traversal](binary-tree-level-order-traversal.md) | BFS with a queue, tracking level |
| [Validate Binary Search Tree](validate-binary-search-tree.md) | Recursive with min/max bounds |
| [Kth Smallest Element in a BST](kth-smallest-element-bst.md) | In-order traversal until kth element |
| [Construct Binary Tree from Preorder and Inorder Traversal](construct-binary-tree.md) | Recursively build tree using the properties of pre/in-order traversals |
| [Binary Tree Maximum Path Sum](binary-tree-maximum-path-sum.md) | Post-order traversal tracking maximum path sums |
| [Serialize and Deserialize Binary Tree](serialize-deserialize-binary-tree.md) | DFS or BFS with special markers for null nodes |

## Tries

| Problem | Approach |
|---------|----------|
| [Implement Trie (Prefix Tree)](implement-trie.md) | Create TrieNode structure with children and end flag |
| [Design Add and Search Words Data Structure](add-search-words.md) | Extend Trie with wildcard search functionality |
| [Word Search II](word-search-ii.md) | Use Trie to efficiently search for all words in the board |

## Heap / Priority Queue

| Problem | Approach |
|---------|----------|
| [Find Median from Data Stream](find-median-data-stream.md) | Maintain two heaps (max-heap for lower half, min-heap for upper half) |

## Backtracking

| Problem | Approach |
|---------|----------|
| [Combination Sum](combination-sum.md) | Recursively try all possible combinations |
| [Word Search](word-search.md) | DFS for each starting character, marking visited cells |

## Graphs

| Problem | Approach |
|---------|----------|
| [Number of Islands](number-of-islands.md) | DFS or BFS to explore connected land |
| [Clone Graph](clone-graph.md) | BFS or DFS with a hash map to track cloned nodes |
| [Pacific Atlantic Water Flow](pacific-atlantic-water-flow.md) | DFS from ocean boundaries, finding common cells |
| [Course Schedule](course-schedule.md) | Topological sort or cycle detection in directed graph |
| [Number of Connected Components in an Undirected Graph](number-of-connected-components.md) | DFS/BFS or Union-Find |
| [Graph Valid Tree](graph-valid-tree.md) | Check if graph has n-1 edges and is connected |

## Advanced Graphs

| Problem | Approach |
|---------|----------|
| [Alien Dictionary](alien-dictionary.md) | Build a directed graph and perform topological sorting |
| [Cheapest Flights Within K Stops](cheapest-flights-k-stops.md) | Dijkstra's algorithm or Bellman-Ford with stops constraint |

## 1-D Dynamic Programming

| Problem | Approach |
|---------|----------|
| [Climbing Stairs](climbing-stairs.md) | DP where each step is sum of ways to reach previous steps |
| [House Robber](house-robber.md) | DP choosing best between adjacent houses |
| [House Robber II](house-robber-ii.md) | Apply House Robber twice on different subsets |
| [Longest Palindromic Substring](longest-palindromic-substring.md) | DP or expand around center to find palindromes |
| [Palindromic Substrings](palindromic-substrings.md) | Similar approach to longest palindromic substring |
| [Decode Ways](decode-ways.md) | DP with character-by-character decisions |
| [Coin Change](coin-change.md) | DP to find minimum number of coins for each amount |
| [Maximum Product Subarray](maximum-product-subarray.md) | Track both min and max products while traversing |
| [Word Break](word-break.md) | DP to check if string can be segmented into words |
| [Longest Increasing Subsequence](longest-increasing-subsequence.md) | DP or patience sort approach |

## 2-D Dynamic Programming

| Problem | Approach |
|---------|----------|
| [Unique Paths](unique-paths.md) | DP calculating ways to reach each cell |
| [Longest Common Subsequence](longest-common-subsequence.md) | DP comparing characters and tracking matches |

## Greedy

| Problem | Approach |
|---------|----------|
| [Maximum Subarray](maximum-subarray.md) | Kadane's algorithm to track local and global maximums |
| [Jump Game](jump-game.md) | Greedy approach tracking furthest index that can be reached |

## Intervals

| Problem | Approach |
|---------|----------|
| [Insert Interval](insert-interval.md) | Insert new interval while merging overlaps |
| [Merge Intervals](merge-intervals.md) | Sort intervals and merge overlapping ones |
| [Non-overlapping Intervals](non-overlapping-intervals.md) | Sort and greedy selection to minimize removals |
| [Meeting Rooms](meeting-rooms.md) | Check for any overlap in sorted intervals |
| [Meeting Rooms II](meeting-rooms-ii.md) | Use min heap or sweep line to track concurrent meetings |

## Math & Geometry

| Problem | Approach |
|---------|----------|
| [Rotate Image](rotate-image.md) | Transpose matrix and reverse rows, or rotate in layers |
| [Spiral Matrix](spiral-matrix.md) | Traverse the matrix in a spiral pattern shrinking boundaries |
| [Set Matrix Zeroes](set-matrix-zeroes.md) | Use first row and column as markers for zeroes |

## Bit Manipulation

| Problem | Approach |
|---------|----------|
| [Number of 1 Bits](number-of-1-bits.md) | Count the number of set bits using bit manipulation |
| [Counting Bits](counting-bits.md) | DP with bit manipulation patterns |
| [Reverse Bits](reverse-bits.md) | Bit manipulation to reverse the binary representation |
| [Missing Number](missing-number.md) | XOR all numbers with their indices to find missing one |
| [Sum of Two Integers](sum-of-two-integers.md) | Use bit manipulation for addition without + operator |