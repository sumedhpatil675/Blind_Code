# Sum of Two Integers

Given two integers a and b, return the sum of the two integers without using the operators + and -.

## Approaches

### 1. Bit Manipulation (Basic)
- **Time complexity**: O(1)
- **Space complexity**: O(1)

#### Python
```python
def getSum(a, b):
    # Python handles integers differently due to infinite integer range
    # We need to mask the result to simulate 32-bit integer arithmetic
    mask = 0xFFFFFFFF
    while b != 0:
        carry = ((a & b) & mask) << 1
        a = (a ^ b) & mask
        b = carry
    
    # Handle negative results (sign extension)
    max_int = 0x7FFFFFFF
    if a > max_int:
        a = ~(a ^ mask)
    
    return a
```

#### JavaScript
```javascript
function getSum(a, b) {
    while (b !== 0) {
        const carry = (a & b) << 1;
        a = a ^ b;
        b = carry;
    }
    return a;
}
```

#### Java
```java
public int getSum(int a, int b) {
    while (b != 0) {
        int carry = a & b;  // Carry is AND of a and b
        a = a ^ b;          // Sum is XOR of a and b
        b = carry << 1;     // Shift carry by one
    }
    return a;
}
```

### 2. Recursive Approach
- **Time complexity**: O(1)
- **Space complexity**: O(1)

#### Python
```python
def getSum(a, b):
    mask = 0xFFFFFFFF
    max_int = 0x7FFFFFFF
    
    def recursive_sum(x, y):
        if y == 0:
            return x if x <= max_int else ~(x ^ mask)
        else:
            return recursive_sum((x ^ y) & mask, ((x & y) << 1) & mask)
    
    return recursive_sum(a, b)
```

#### JavaScript
```javascript
function getSum(a, b) {
    if (b === 0) {
        return a;
    }
    return getSum(a ^ b, (a & b) << 1);
}
```

#### Java
```java
public int getSum(int a, int b) {
    return b == 0 ? a : getSum(a ^ b, (a & b) << 1);
}
```

### 3. Iterative Approach with Mask and Check
- **Time complexity**: O(1)
- **Space complexity**: O(1)

#### Python
```python
def getSum(a, b):
    # 32-bit mask in hexadecimal
    mask = 0xFFFFFFFF
    
    # Ensure a and b are 32-bit integers
    a = a & mask
    b = b & mask
    
    while b != 0:
        carry = ((a & b) & mask) << 1
        a = (a ^ b) & mask
        b = carry
    
    # If a is negative in 32-bit, convert it back
    if a > 0x7FFFFFFF:
        a = ~(a ^ mask)
    
    return a
```

#### JavaScript
```javascript
function getSum(a, b) {
    const mask = 0xFFFFFFFF;
    
    // Ensure a and b are 32-bit integers
    a &= mask;
    b &= mask;
    
    while (b !== 0) {
        const carry = ((a & b) & mask) << 1;
        a = (a ^ b) & mask;
        b = carry;
    }
    
    // If a is negative in 32-bit, convert it back
    if (a > 0x7FFFFFFF) {
        a = ~(a ^ mask);
    }
    
    return a;
}
```

#### Java
```java
public int getSum(int a, int b) {
    while (b != 0) {
        int carry = a & b;
        a = a ^ b;
        b = carry << 1;
    }
    return a;
}
```

# Addition Without the `+` Operator

This function (`getSum`) adds two integers without using the `+` operator by using bit manipulation techniques.

## Explanation with ASCII Diagrams

Let's follow the execution with an example: `getSum(5, 3)`

### Starting Values
```
a = 5 = 101 (binary)
b = 3 = 011 (binary)
```

### Iteration 1

#### Step 1: Calculate Carry
```
a & b = 101 & 011 = 001 (1 in decimal)

    101  (a = 5)
  & 011  (b = 3)
  -----
    001  (carry bits before shifting)
```

```
(a & b) << 1 = 001 << 1 = 010 (2 in decimal)

    001  (carry before shift)
  << 1
  -----
    010  (carry after shift = 2)
```

#### Step 2: Sum Without Carry (Using XOR)
```
a ^ b = 101 ^ 011 = 110 (6 in decimal)

    101  (a = 5)
  ^ 011  (b = 3)
  -----
    110  (sum without carry = 6)
```

#### Step 3: Update Variables
```
a = 110 (6 in decimal)
b = 010 (2 in decimal)
```

### Iteration 2

#### Step 1: Calculate Carry
```
a & b = 110 & 010 = 010 (2 in decimal)

    110  (a = 6)
  & 010  (b = 2)
  -----
    010  (carry bits before shifting)
```

```
(a & b) << 1 = 010 << 1 = 100 (4 in decimal)

    010  (carry before shift)
  << 1
  -----
    100  (carry after shift = 4)
```

#### Step 2: Sum Without Carry (Using XOR)
```
a ^ b = 110 ^ 010 = 100 (4 in decimal)

    110  (a = 6)
  ^ 010  (b = 2)
  -----
    100  (sum without carry = 4)
```

#### Step 3: Update Variables
```
a = 100 (4 in decimal)
b = 100 (4 in decimal)
```

### Iteration 3

#### Step 1: Calculate Carry
```
a & b = 100 & 100 = 100 (4 in decimal)

    100  (a = 4)
  & 100  (b = 4)
  -----
    100  (carry bits before shifting)
```

```
(a & b) << 1 = 100 << 1 = 1000 (8 in decimal)

    100  (carry before shift)
  << 1
  -----
   1000  (carry after shift = 8)
```

#### Step 2: Sum Without Carry (Using XOR)
```
a ^ b = 100 ^ 100 = 000 (0 in decimal)

    100  (a = 4)
  ^ 100  (b = 4)
  -----
    000  (sum without carry = 0)
```

#### Step 3: Update Variables
```
a = 000 (0 in decimal)
b = 1000 (8 in decimal)
```

### Iteration 4

#### Step 1: Calculate Carry
```
a & b = 000 & 1000 = 0000 (0 in decimal)

    0000  (a = 0)
  & 1000  (b = 8)
  ------
    0000  (carry bits before shifting)
```

```
(a & b) << 1 = 0000 << 1 = 0000 (0 in decimal)

    0000  (carry before shift)
  << 1
  ------
    0000  (carry after shift = 0)
```

#### Step 2: Sum Without Carry (Using XOR)
```
a ^ b = 0000 ^ 1000 = 1000 (8 in decimal)

    0000  (a = 0)
  ^ 1000  (b = 8)
  ------
    1000  (sum without carry = 8)
```

#### Step 3: Update Variables
```
a = 1000 (8 in decimal)
b = 0000 (0 in decimal)
```

### Termination
Since `b = 0`, the loop terminates and returns `a = 8`, which is the correct sum of 5 + 3!

## Flowchart Representation

```
┌───────────────┐
│ Start getSum  │
│  (a=5, b=3)   │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│  while loop   │
│   (b != 0)    │──────────┐
└───────┬───────┘          │
        │                  │
        ▼                  │
┌───────────────┐          │
│ carry = a & b │ 001      │
│ (then << 1)   │ 010      │
└───────┬───────┘          │
        │                  │
        ▼                  │
┌───────────────┐          │
│  a = a ^ b    │ 110      │
└───────┬───────┘          │
        │                  │
        ▼                  │
┌───────────────┐          │
│  b = carry    │ 010      │
└───────┬───────┘          │
        │                  │
        ▼                  │
┌───────────────┐          │
│  while loop   │          │
│   (b != 0)    │──────────┤
└───────┬───────┘          │
        │                  │
        ▼                  │
┌───────────────┐          │
│ carry = a & b │ 010      │
│ (then << 1)   │ 100      │
└───────┬───────┘          │
        │                  │
        ▼                  │
┌───────────────┐          │
│  a = a ^ b    │ 100      │
└───────┬───────┘          │
        │                  │
        ▼                  │
┌───────────────┐          │
│  b = carry    │ 100      │
└───────┬───────┘          │
        │                  │
        ▼                  │
┌───────────────┐          │
│  while loop   │          │
│   (b != 0)    │──────────┤
└───────┬───────┘          │
        │                  │
        ▼                  │
       ... (continue iterations) ...
        │                  │
        ▼                  │
┌───────────────┐          │
│  while loop   │          │
│   (b = 0)     │──────────┘
└───────┬───────┘
        │
        ▼
┌───────────────┐
│   return a    │ 8 (result!)
└───────────────┘
```

## Summary

1. **XOR (`^`)** calculates the sum without considering carry
2. **AND (`&`)** followed by left shift (`<<`) calculates the carry
3. The process repeats until no carry is left (b = 0)
4. This mimics the standard school addition method but with binary operations

The algorithm has a time complexity of O(log n) where n is the largest number, as in the worst case we need to process all bits.


### 4. Using Bit Manipulation with XOR and Carry
- **Time complexity**: O(1)
- **Space complexity**: O(1)

#### Python
```python
def getSum(a, b):
    x, y = abs(a), abs(b)
    
    # Ensure a >= b
    if x < y:
        return getSum(b, a)
    
    # Handle the sign
    sign = 1 if a > 0 else -1
    
    if a * b >= 0:
        # Both a and b have the same sign
        # Use XOR to add without carry, AND with left shift for the carry
        while y:
            x, y = x ^ y, (x & y) << 1
    else:
        # a and b have different signs
        # Use XOR to subtract without borrow, AND with complement and left shift for the borrow
        while y:
            x, y = x ^ y, ((~x) & y) << 1
    
    return x * sign
```

#### JavaScript
```javascript
function getSum(a, b) {
    let x = Math.abs(a);
    let y = Math.abs(b);
    
    // Ensure x >= y
    if (x < y) {
        return getSum(b, a);
    }
    
    // Handle the sign
    const sign = a > 0 ? 1 : -1;
    
    if (a * b >= 0) {
        // Both a and b have the same sign
        while (y !== 0) {
            const temp = x ^ y;
            y = (x & y) << 1;
            x = temp;
        }
    } else {
        // a and b have different signs
        while (y !== 0) {
            const temp = x ^ y;
            y = ((~x) & y) << 1;
            x = temp;
        }
    }
    
    return sign * x;
}
```

#### Java
```java
public int getSum(int a, int b) {
    int x = Math.abs(a);
    int y = Math.abs(b);
    
    // Ensure x >= y
    if (x < y) {
        return getSum(b, a);
    }
    
    // Handle the sign
    int sign = a > 0 ? 1 : -1;
    
    if (a * b >= 0) {
        // Both a and b have the same sign
        while (y != 0) {
            int temp = x ^ y;
            y = (x & y) << 1;
            x = temp;
        }
    } else {
        // a and b have different signs
        while (y != 0) {
            int temp = x ^ y;
            y = ((~x) & y) << 1;
            x = temp;
        }
    }
    
    return sign * x;
}
```
