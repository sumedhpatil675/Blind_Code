# Longest Increasing Subsequence

Given an integer array nums, return the length of the longest strictly increasing subsequence.

## Approaches

### 1. Recursion
- **Time complexity**: O(2^n)
- **Space complexity**: O(n)

#### Python
```python
def lengthOfLIS(nums):
    def dfs(i, j):
        if i == len(nums):
            return 0
        
        LIS = dfs(i + 1, j)  # not include
        if j == -1 or nums[j] < nums[i]:
            LIS = max(LIS, 1 + dfs(i + 1, i))  # include
            
        return LIS
    
    return dfs(0, -1)
```

#### JavaScript
```javascript
function lengthOfLIS(nums) {
    function dfs(i, j) {
        if (i === nums.length) {
            return 0;
        }
        
        let LIS = dfs(i + 1, j);  // not include
        if (j === -1 || nums[j] < nums[i]) {
            LIS = Math.max(LIS, 1 + dfs(i + 1, i));  // include
        }
        
        return LIS;
    }
    
    return dfs(0, -1);
}
```

#### Java
```java
public int lengthOfLIS(int[] nums) {
    return dfs(nums, 0, -1);
}

private int dfs(int[] nums, int i, int j) {
    if (i == nums.length) {
        return 0;
    }
    
    int LIS = dfs(nums, i + 1, j);  // not include
    if (j == -1 || nums[j] < nums[i]) {
        LIS = Math.max(LIS, 1 + dfs(nums, i + 1, i));  // include
    }
    
    return LIS;
}
```

### 2. Dynamic Programming (Top-Down)
- **Time complexity**: O(n²)
- **Space complexity**: O(n²)

#### Python
```python
def lengthOfLIS(nums):
    n = len(nums)
    memo = [[-1] * (n + 1) for _ in range(n)]
    
    def dfs(i, j):
        if i == n:
            return 0
        
        if memo[i][j + 1] != -1:
            return memo[i][j + 1]
        
        LIS = dfs(i + 1, j)
        if j == -1 or nums[j] < nums[i]:
            LIS = max(LIS, 1 + dfs(i + 1, i))
            
        memo[i][j + 1] = LIS
        return LIS
    
    return dfs(0, -1)
```

#### JavaScript
```javascript
function lengthOfLIS(nums) {
    const n = nums.length;
    const memo = Array(n).fill().map(() => Array(n + 1).fill(-1));
    
    function dfs(i, j) {
        if (i === n) {
            return 0;
        }
        
        if (memo[i][j + 1] !== -1) {
            return memo[i][j + 1];
        }
        
        let LIS = dfs(i + 1, j);
        if (j === -1 || nums[j] < nums[i]) {
            LIS = Math.max(LIS, 1 + dfs(i + 1, i));
        }
        
        memo[i][j + 1] = LIS;
        return LIS;
    }
    
    return dfs(0, -1);
}
```

#### Java
```java
public int lengthOfLIS(int[] nums) {
    int n = nums.length;
    int[][] memo = new int[n][n + 1];
    
    for (int[] row : memo) {
        Arrays.fill(row, -1);
    }
    
    return dfs(nums, 0, -1, memo);
}

private int dfs(int[] nums, int i, int j, int[][] memo) {
    if (i == nums.length) {
        return 0;
    }
    
    if (memo[i][j + 1] != -1) {
        return memo[i][j + 1];
    }
    
    int LIS = dfs(nums, i + 1, j, memo);
    if (j == -1 || nums[j] < nums[i]) {
        LIS = Math.max(LIS, 1 + dfs(nums, i + 1, i, memo));
    }
    
    memo[i][j + 1] = LIS;
    return LIS;
}
```

### 3. Dynamic Programming (Bottom-Up) ✅✅✅
- **Time complexity**: O(n²)
- **Space complexity**: O(n)


#### Python
```python
def lengthOfLIS(nums):
    LIS = [1] * len(nums)
    
    for i in range(len(nums) - 1, -1, -1):
        for j in range(i + 1, len(nums)):
            if nums[i] < nums[j]:
                LIS[i] = max(LIS[i], 1 + LIS[j])
    
    return max(LIS)
```

#### JavaScript
```javascript
function lengthOfLIS(nums) {
    const LIS = Array(nums.length).fill(1);
    
    for (let i = nums.length - 1; i >= 0; i--) {
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[i] < nums[j]) {
                LIS[i] = Math.max(LIS[i], 1 + LIS[j]);
            }
        }
    }
    
    return Math.max(...LIS);
}
```

#### Java
```java
public int lengthOfLIS(int[] nums) {
    int[] LIS = new int[nums.length];
    Arrays.fill(LIS, 1);
    
    for (int i = nums.length - 1; i >= 0; i--) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] < nums[j]) {
                LIS[i] = Math.max(LIS[i], 1 + LIS[j]);
            }
        }
    }
    
    int max = 0;
    for (int val : LIS) {
        max = Math.max(max, val);
    }
    
    return max;
}
```

## DP Table Trace

Input array: `nums = [10, 9, 2, 5, 3, 7, 101, 18]`

The DP table (LIS array) initially has all values set to 1:

```
Index:  0   1   2   3   4   5   6    7
nums:   10  9   2   5   3   7   101  18
LIS:    1   1   1   1   1   1   1    1
```

### Working backwards through the array:

**For i = 7 (nums[7] = 18):**
No elements to the right, so LIS[7] remains 1.

```
Index:  0   1   2   3   4   5   6    7
nums:   10  9   2   5   3   7   101  18
LIS:    1   1   1   1   1   1   1    1
```

**For i = 6 (nums[6] = 101):**
- j = 7, nums[6] = 101 > nums[7] = 18, so no update.
- LIS[6] remains 1.

```
Index:  0   1   2   3   4   5   6    7
nums:   10  9   2   5   3   7   101  18
LIS:    1   1   1   1   1   1   1    1
```

**For i = 5 (nums[5] = 7):**
- j = 6, nums[5] = 7 < nums[6] = 101, so LIS[5] = max(1, 1+1) = 2
- j = 7, nums[5] = 7 < nums[7] = 18, so LIS[5] = max(2, 1+1) = 2

```
Index:  0   1   2   3   4   5   6    7
nums:   10  9   2   5   3   7   101  18
LIS:    1   1   1   1   1   2   1    1
```

**For i = 4 (nums[4] = 3):**
- j = 5, nums[4] = 3 < nums[5] = 7, so LIS[4] = max(1, 1+2) = 3
- j = 6, nums[4] = 3 < nums[6] = 101, so LIS[4] = max(3, 1+1) = 3
- j = 7, nums[4] = 3 < nums[7] = 18, so LIS[4] = max(3, 1+1) = 3

```
Index:  0   1   2   3   4   5   6    7
nums:   10  9   2   5   3   7   101  18
LIS:    1   1   1   1   3   2   1    1
```

**For i = 3 (nums[3] = 5):**
- j = 4, nums[3] = 5 > nums[4] = 3, so no update.
- j = 5, nums[3] = 5 < nums[5] = 7, so LIS[3] = max(1, 1+2) = 3
- j = 6, nums[3] = 5 < nums[6] = 101, so LIS[3] = max(3, 1+1) = 3
- j = 7, nums[3] = 5 < nums[7] = 18, so LIS[3] = max(3, 1+1) = 3

```
Index:  0   1   2   3   4   5   6    7
nums:   10  9   2   5   3   7   101  18
LIS:    1   1   1   3   3   2   1    1
```

**For i = 2 (nums[2] = 2):**
- j = 3, nums[2] = 2 < nums[3] = 5, so LIS[2] = max(1, 1+3) = 4
- j = 4, nums[2] = 2 < nums[4] = 3, so LIS[2] = max(4, 1+3) = 4
- j = 5, nums[2] = 2 < nums[5] = 7, so LIS[2] = max(4, 1+2) = 4
- j = 6, nums[2] = 2 < nums[6] = 101, so LIS[2] = max(4, 1+1) = 4
- j = 7, nums[2] = 2 < nums[7] = 18, so LIS[2] = max(4, 1+1) = 4

```
Index:  0   1   2   3   4   5   6    7
nums:   10  9   2   5   3   7   101  18
LIS:    1   1   4   3   3   2   1    1
```

**For i = 1 (nums[1] = 9):**
- j = 2, nums[1] = 9 > nums[2] = 2, so no update.
- j = 3, nums[1] = 9 > nums[3] = 5, so no update.
- j = 4, nums[1] = 9 > nums[4] = 3, so no update.
- j = 5, nums[1] = 9 > nums[5] = 7, so no update.
- j = 6, nums[1] = 9 < nums[6] = 101, so LIS[1] = max(1, 1+1) = 2
- j = 7, nums[1] = 9 < nums[7] = 18, so LIS[1] = max(2, 1+1) = 2

```
Index:  0   1   2   3   4   5   6    7
nums:   10  9   2   5   3   7   101  18
LIS:    1   2   4   3   3   2   1    1
```

**For i = 0 (nums[0] = 10):**
- j = 1, nums[0] = 10 > nums[1] = 9, so no update.
- j = 2, nums[0] = 10 > nums[2] = 2, so no update.
- j = 3, nums[0] = 10 > nums[3] = 5, so no update.
- j = 4, nums[0] = 10 > nums[4] = 3, so no update.
- j = 5, nums[0] = 10 > nums[5] = 7, so no update.
- j = 6, nums[0] = 10 < nums[6] = 101, so LIS[0] = max(1, 1+1) = 2
- j = 7, nums[0] = 10 < nums[7] = 18, so LIS[0] = max(2, 1+1) = 2

```
Index:  0   1   2   3   4   5   6    7
nums:   10  9   2   5   3   7   101  18
LIS:    2   2   4   3   3   2   1    1
```

## Result
The maximum value in the LIS array is 4, so the length of the longest increasing subsequence is 4.

Possible longest increasing subsequences:
- [2, 5, 7, 101]
- [2, 3, 7, 101]

- 

### 4. Binary Search
- **Time complexity**: O(n log n)
- **Space complexity**: O(n)

#### Python
```python
from bisect import bisect_left

def lengthOfLIS(nums):
    dp = []
    
    for num in nums:
        i = bisect_left(dp, num)
        if i == len(dp):
            dp.append(num)
        else:
            dp[i] = num
            
    return len(dp)
```

#### JavaScript
```javascript
function lengthOfLIS(nums) {
    const dp = [];
    
    for (const num of nums) {
        // Find the index of the smallest number in dp that is >= num
        let left = 0, right = dp.length;
        while (left < right) {
            const mid = Math.floor((left + right) / 2);
            if (dp[mid] < num) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        // If num is greater than all elements in dp, append it
        if (left === dp.length) {
            dp.push(num);
        } else {
            dp[left] = num;
        }
    }
    
    return dp.length;
}
```

#### Java
```java
public int lengthOfLIS(int[] nums) {
    ArrayList<Integer> dp = new ArrayList<>();
    
    for (int num : nums) {
        int i = Collections.binarySearch(dp, num);
        if (i < 0) {
            i = -(i + 1);
        }
        
        if (i == dp.size()) {
            dp.add(num);
        } else {
            dp.set(i, num);
        }
    }
    
    return dp.size();
}
```

### 5. Segment Tree (Advanced)
- **Time complexity**: O(n log n)
- **Space complexity**: O(n)

#### Python
```python
from bisect import bisect_left

class SegmentTree:
    def __init__(self, N):
        self.n = N
        while (self.n & (self.n - 1)) != 0:
            self.n += 1
        self.tree = [0] * (2 * self.n)
    
    def update(self, i, val):
        self.tree[self.n + i] = val
        j = (self.n + i) >> 1
        while j >= 1:
            self.tree[j] = max(self.tree[j << 1], self.tree[j << 1 | 1])
            j >>= 1
    
    def query(self, l, r):
        if l > r:
            return 0
        res = float('-inf')
        l += self.n
        r += self.n + 1
        while l < r:
            if l & 1:
                res = max(res, self.tree[l])
                l += 1
            if r & 1:
                r -= 1
                res = max(res, self.tree[r])
            l >>= 1
            r >>= 1
        return res

def lengthOfLIS(nums):
    def compress(arr):
        sortedArr = sorted(set(arr))
        order = []
        for num in arr:
            order.append(bisect_left(sortedArr, num))
        return order
    
    nums = compress(nums)
    n = len(nums)
    segTree = SegmentTree(n)
    LIS = 0
    
    for num in nums:
        curLIS = segTree.query(0, num - 1) + 1
        segTree.update(num, curLIS)
        LIS = max(LIS, curLIS)
    
    return LIS
```

#### JavaScript
```javascript
class SegmentTree {
    constructor(N) {
        this.n = N;
        while ((this.n & (this.n - 1)) !== 0) {
            this.n += 1;
        }
        this.tree = Array(2 * this.n).fill(0);
    }
    
    update(i, val) {
        this.tree[this.n + i] = val;
        let j = (this.n + i) >> 1;
        while (j >= 1) {
            this.tree[j] = Math.max(this.tree[j << 1], this.tree[(j << 1) | 1]);
            j >>= 1;
        }
    }
    
    query(l, r) {
        if (l > r) return 0;
        let res = Number.NEGATIVE_INFINITY;
        l += this.n;
        r += this.n + 1;
        while (l < r) {
            if (l & 1) {
                res = Math.max(res, this.tree[l]);
                l += 1;
            }
            if (r & 1) {
                r -= 1;
                res = Math.max(res, this.tree[r]);
            }
            l >>= 1;
            r >>= 1;
        }
        return res;
    }
}

function lengthOfLIS(nums) {
    function compress(arr) {
        const sortedArr = [...new Set(arr)].sort((a, b) => a - b);
        return arr.map(num => {
            let left = 0, right = sortedArr.length;
            while (left < right) {
                const mid = Math.floor((left + right) / 2);
                if (sortedArr[mid] < num) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            return left;
        });
    }
    
    const compressedNums = compress(nums);
    const n = nums.length;
    const segTree = new SegmentTree(n);
    let LIS = 0;
    
    for (const num of compressedNums) {
        const curLIS = segTree.query(0, num - 1) + 1;
        segTree.update(num, curLIS);
        LIS = Math.max(LIS, curLIS);
    }
    
    return LIS;
}
```

#### Java
```java
class SegmentTree {
    int[] tree;
    int n;
    
    public SegmentTree(int N) {
        n = N;
        while ((n & (n - 1)) != 0) {
            n++;
        }
        tree = new int[2 * n];
    }
    
    public void update(int i, int val) {
        tree[n + i] = val;
        int j = (n + i) >> 1;
        while (j >= 1) {
            tree[j] = Math.max(tree[j << 1], tree[(j << 1) | 1]);
            j >>= 1;
        }
    }
    
    public int query(int l, int r) {
        if (l > r) return 0;
        int res = Integer.MIN_VALUE;
        l += n;
        r += n + 1;
        while (l < r) {
            if ((l & 1) != 0) {
                res = Math.max(res, tree[l]);
                l++;
            }
            if ((r & 1) != 0) {
                r--;
                res = Math.max(res, tree[r]);
            }
            l >>= 1;
            r >>= 1;
        }
        return res;
    }
}

public int lengthOfLIS(int[] nums) {
    int[] compressedNums = compress(nums);
    int n = nums.length;
    SegmentTree segTree = new SegmentTree(n);
    int LIS = 0;
    
    for (int num : compressedNums) {
        int curLIS = segTree.query(0, num - 1) + 1;
        segTree.update(num, curLIS);
        LIS = Math.max(LIS, curLIS);
    }
    
    return LIS;
}

private int[] compress(int[] nums) {
    TreeSet<Integer> set = new TreeSet<>();
    for (int num : nums) {
        set.add(num);
    }
    
    HashMap<Integer, Integer> map = new HashMap<>();
    int idx = 0;
    for (int num : set) {
        map.put(num, idx++);
    }
    
    int[] result = new int[nums.length];
    for (int i = 0; i < nums.length; i++) {
        result[i] = map.get(nums[i]);
    }
    
    return result;
}
```
