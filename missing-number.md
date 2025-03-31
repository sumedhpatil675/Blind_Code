# Missing Number

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

## Approaches

### 1. Sorting
- **Time complexity**: O(n log n)
- **Space complexity**: O(1) or O(n) depending on the sorting algorithm

#### Python
```python
def missingNumber(nums):
    nums.sort()
    for i in range(len(nums)):
        if nums[i] != i:
            return i
    return len(nums)
```

#### JavaScript
```javascript
function missingNumber(nums) {
    nums.sort((a, b) => a - b);
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== i) {
            return i;
        }
    }
    return nums.length;
}
```

#### Java
```java
public int missingNumber(int[] nums) {
    Arrays.sort(nums);
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != i) {
            return i;
        }
    }
    return nums.length;
}
```

### 2. Hash Set
- **Time complexity**: O(n)
- **Space complexity**: O(n)

#### Python
```python
def missingNumber(nums):
    num_set = set(nums)
    for i in range(len(nums) + 1):
        if i not in num_set:
            return i
    return -1  # This line should never execute
```

#### JavaScript
```javascript
function missingNumber(nums) {
    const numSet = new Set(nums);
    for (let i = 0; i <= nums.length; i++) {
        if (!numSet.has(i)) {
            return i;
        }
    }
    return -1;  // This line should never execute
}
```

#### Java
```java
public int missingNumber(int[] nums) {
    Set<Integer> numSet = new HashSet<>();
    for (int num : nums) {
        numSet.add(num);
    }
    
    for (int i = 0; i <= nums.length; i++) {
        if (!numSet.contains(i)) {
            return i;
        }
    }
    return -1;  // This line should never execute
}
```

### 3. Bit Manipulation (XOR)
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def missingNumber(nums):
    missing = len(nums)
    for i, num in enumerate(nums):
        missing ^= i ^ num
    return missing
```

#### JavaScript
```javascript
function missingNumber(nums) {
    let missing = nums.length;
    for (let i = 0; i < nums.length; i++) {
        missing ^= i ^ nums[i];
    }
    return missing;
}
```

#### Java
```java
public int missingNumber(int[] nums) {
    int missing = nums.length;
    for (int i = 0; i < nums.length; i++) {
        missing ^= i ^ nums[i];
    }
    return missing;
}
```

### 4. Math (Sum Formula)
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def missingNumber(nums):
    n = len(nums)
    expected_sum = n * (n + 1) // 2
    actual_sum = sum(nums)
    return expected_sum - actual_sum
```

#### JavaScript
```javascript
function missingNumber(nums) {
    const n = nums.length;
    const expectedSum = (n * (n + 1)) / 2;
    const actualSum = nums.reduce((sum, num) => sum + num, 0);
    return expectedSum - actualSum;
}
```

#### Java
```java
public int missingNumber(int[] nums) {
    int n = nums.length;
    int expectedSum = n * (n + 1) / 2;
    int actualSum = 0;
    for (int num : nums) {
        actualSum += num;
    }
    return expectedSum - actualSum;
}
```

### 5. Gauss Formula (Optimized Math)
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def missingNumber(nums):
    n = len(nums)
    return n * (n + 1) // 2 - sum(nums)
```

#### JavaScript
```javascript
function missingNumber(nums) {
    const n = nums.length;
    const expectedSum = (n * (n + 1)) / 2;
    const actualSum = nums.reduce((sum, num) => sum + num, 0);
    return expectedSum - actualSum;
}
```

#### Java
```java
public int missingNumber(int[] nums) {
    int n = nums.length;
    int sum = 0;
    for (int num : nums) {
        sum += num;
    }
    return n * (n + 1) / 2 - sum;
}
```

### 6. Iterative Approach
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def missingNumber(nums):
    res = len(nums)
    for i in range(len(nums)):
        res += i - nums[i]
    return res
```

#### JavaScript
```javascript
function missingNumber(nums) {
    let res = nums.length;
    for (let i = 0; i < nums.length; i++) {
        res += i - nums[i];
    }
    return res;
}
```

#### Java
```java
public int missingNumber(int[] nums) {
    int res = nums.length;
    for (int i = 0; i < nums.length; i++) {
        res += i - nums[i];
    }
    return res;
}
```