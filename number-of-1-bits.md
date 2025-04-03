# Number of 1 Bits

Write a function that takes the binary representation of an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

## Approaches

### 1. Bit Mask - I
- **Time complexity**: O(1)
- **Space complexity**: O(1)

#### Python
```python
def hammingWeight(n):
    res = 0
    for i in range(32):
        if (1 << i) & n:
            res += 1
    return res
```

#### JavaScript
```javascript
function hammingWeight(n) {
    let res = 0;
    for (let i = 0; i < 32; i++) {
        if ((1 << i) & n) {
            res += 1;
        }
    }
    return res;
}
```

#### Java
```java
public int hammingWeight(int n) {
    int res = 0;
    for (int i = 0; i < 32; i++) {
        if ((n & (1 << i)) != 0) {
            res++;
        }
    }
    return res;
}
```

### 2. Bit Mask - II
- **Time complexity**: O(1)
- **Space complexity**: O(1)

## Step-by-Step Breakdown

1. **Checking the rightmost bit**:
   - If n is odd → n % 2 → gives 1 (represents the rightmost bit)
   - If n is even → n % 2 → gives 0 (represents the rightmost bit)
   - We add this value (0 or 1) to our result counter `res`

2. **Discarding the processed bit**:
   - We use `n >> 1` to right-shift the number, effectively removing the rightmost bit
   - This prepares the next bit for examination in the next iteration

3. **Repeat until done**:
   - The process continues until n becomes 0 (no more bits to examine)

## Example: n = 11 (binary: 1011)

| Iteration | n (decimal) | n (binary) | n % 2 | res |
|-----------|-------------|------------|-------|-----|
| Initial   | 11          | 1011       | -     | 0   |
| 1         | 11          | 1011       | 1     | 1   |
| 2         | 5           | 101        | 1     | 2   |
| 3         | 2           | 10         | 0     | 2   |
| 4         | 1           | 1          | 1     | 3   |
| 5         | 0           | 0          | -     | 3   |

Final result: 3 (which is the number of 1s in the binary representation of 11)


#### Python
```python
def hammingWeight(n):
    res = 0
    while n:
        res += n %2 
        n >>= 1
    return res
```

#### JavaScript
```javascript
function hammingWeight(n) {
    let res = 0;
    while (n !== 0) {
        res += n % 2;
        n >>>= 1;  // Unsigned right shift
    }
    return res;
}
```

#### Java
```java
public int hammingWeight(int n) {
    int res = 0;
    while (n != 0) {
        res += n % 2;
        n >>>= 1;  // Unsigned right shift
    }
    return res;
}
```

### 3. Bit Manipulation (Optimal)
- **Time complexity**: O(1)
- **Space complexity**: O(1)

#### Python
```python
def hammingWeight(n):
    res = 0
    while n:
        n &= (n - 1)  # Clear the least significant 1 bit
        res += 1
    return res
```

#### JavaScript
```javascript
function hammingWeight(n) {
    let res = 0;
    while (n !== 0) {
        n &= (n - 1);  // Clear the least significant 1 bit
        res += 1;
    }
    return res;
}
```

#### Java
```java
public int hammingWeight(int n) {
    int res = 0;
    while (n != 0) {
        n &= (n - 1);  // Clear the least significant 1 bit
        res++;
    }
    return res;
}
```

### 4. Built-In Function
- **Time complexity**: O(1)
- **Space complexity**: O(1)

#### Python
```python
def hammingWeight(n):
    return bin(n).count('1')
```

#### JavaScript
```javascript
function hammingWeight(n) {
    return n.toString(2).replace(/0/g, '').length;
}
```

#### Java
```java
public int hammingWeight(int n) {
    return Integer.bitCount(n);
}
```
