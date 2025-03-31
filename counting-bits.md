# Counting Bits

Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.

## Approaches

### 1. Bit Manipulation - I
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def countBits(n):
    res = []
    for num in range(n + 1):
        count = 0
        while num:
            count += num & 1
            num >>= 1
        res.append(count)
    return res
```

#### JavaScript
```javascript
function countBits(n) {
    const res = [];
    for (let num = 0; num <= n; num++) {
        let count = 0;
        let temp = num;
        while (temp > 0) {
            count += temp & 1;
            temp >>= 1;
        }
        res.push(count);
    }
    return res;
}
```

#### Java
```java
public int[] countBits(int n) {
    int[] res = new int[n + 1];
    for (int i = 0; i <= n; i++) {
        int count = 0;
        int num = i;
        while (num > 0) {
            count += num & 1;
            num >>= 1;
        }
        res[i] = count;
    }
    return res;
}
```

### 2. Bit Manipulation - II
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def countBits(n):
    res = [0] * (n + 1)
    for i in range(1, n + 1):
        num = i
        while num != 0:
            res[i] += 1
            num &= (num - 1)  # Clear the least significant 1 bit
    return res
```

#### JavaScript
```javascript
function countBits(n) {
    const res = Array(n + 1).fill(0);
    for (let i = 1; i <= n; i++) {
        let num = i;
        while (num !== 0) {
            res[i]++;
            num &= (num - 1);  // Clear the least significant 1 bit
        }
    }
    return res;
}
```

#### Java
```java
public int[] countBits(int n) {
    int[] res = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        int num = i;
        while (num != 0) {
            res[i]++;
            num &= (num - 1);  // Clear the least significant 1 bit
        }
    }
    return res;
}
```

### 3. In-Built Function
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def countBits(n):
    return [bin(i).count('1') for i in range(n + 1)]
```

#### JavaScript
```javascript
function countBits(n) {
    const res = [];
    for (let i = 0; i <= n; i++) {
        res.push(i.toString(2).replace(/0/g, '').length);
    }
    return res;
}
```

#### Java
```java
public int[] countBits(int n) {
    int[] res = new int[n + 1];
    for (int i = 0; i <= n; i++) {
        res[i] = Integer.bitCount(i);
    }
    return res;
}
```

### 4. Dynamic Programming (Using Offset)
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def countBits(n):
    dp = [0] * (n + 1)
    offset = 1
    
    for i in range(1, n + 1):
        if offset * 2 == i:
            offset = i
        dp[i] = 1 + dp[i - offset]
    
    return dp
```

#### JavaScript
```javascript
function countBits(n) {
    const dp = Array(n + 1).fill(0);
    let offset = 1;
    
    for (let i = 1; i <= n; i++) {
        if (offset * 2 === i) {
            offset = i;
        }
        dp[i] = 1 + dp[i - offset];
    }
    
    return dp;
}
```

#### Java
```java
public int[] countBits(int n) {
    int[] dp = new int[n + 1];
    int offset = 1;
    
    for (int i = 1; i <= n; i++) {
        if (offset * 2 == i) {
            offset = i;
        }
        dp[i] = 1 + dp[i - offset];
    }
    
    return dp;
}
```

### 5. Dynamic Programming (Optimal)
- **Time complexity**: O(n)
- **Space complexity**: O(1)

#### Python
```python
def countBits(n):
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        dp[i] = dp[i >> 1] + (i & 1)
    return dp
```

#### JavaScript
```javascript
function countBits(n) {
    const dp = Array(n + 1).fill(0);
    for (let i = 1; i <= n; i++) {
        dp[i] = dp[i >> 1] + (i & 1);
    }
    return dp;
}
```

#### Java
```java
public int[] countBits(int n) {
    int[] dp = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        dp[i] = dp[i >> 1] + (i & 1);
    }
    return dp;
}
```