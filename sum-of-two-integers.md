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