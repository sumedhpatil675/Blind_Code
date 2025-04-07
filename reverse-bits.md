# Reverse Bits

Reverse bits of a given 32 bits unsigned integer.

## Approaches

### 1. Brute Force
- **Time complexity**: O(1)
- **Space complexity**: O(1)

#### Python
```python
def reverseBits(n):
    binary = bin(n)[2:].zfill(32)  # Convert to binary and pad to 32 bits
    reversed_binary = binary[::-1]  # Reverse the binary string
    return int(reversed_binary, 2)  # Convert back to integer
```

#### JavaScript
```javascript
function reverseBits(n) {
    // Convert to binary string, pad to 32 bits, reverse, and convert back to integer
    const binary = n.toString(2).padStart(32, '0');
    const reversed = binary.split('').reverse().join('');
    return parseInt(reversed, 2);
}
```

#### Java
```java
public int reverseBits(int n) {
    // Java doesn't have straightforward binary string conversion like Python
    int result = 0;
    for (int i = 0; i < 32; i++) {
        // Shift result left to make room for the next bit
        result <<= 1;
        // Add the least significant bit of n to result
        result |= (n & 1);
        // Shift n right to process the next bit
        n >>>= 1;
    }
    return result;
}
```

### 2. Bit Manipulation ✅✅✅

- **Time complexity**: O(1)
- **Space complexity**: O(1)

#### Python
```python
def reverseBits(n):
    res = 0
    for i in range(32):
        bit = (n >> i) & 1  # Extract the i-th bit
        res += (bit << (31 - i))  # Set the (31-i)-th bit in result
    return res
```

#### JavaScript
```javascript
function reverseBits(n) {
    let res = 0;
    for (let i = 0; i < 32; i++) {
        const bit = (n >> i) & 1;  // Extract the i-th bit
        res += (bit << (31 - i));  // Set the (31-i)-th bit in result
    }
    return res >>> 0;  // Force unsigned 32-bit integer
}
```

#### Java
```java
public int reverseBits(int n) {
    int res = 0;
    for (int i = 0; i < 32; i++) {
        int bit = (n >> i) & 1;  // Extract the i-th bit
        res |= (bit << (31 - i));  // Set the (31-i)-th bit in result
    }
    return res;
}
```

### 3. Bit Manipulation (Optimal) - Divide and Conquer
- **Time complexity**: O(1)
- **Space complexity**: O(1)

#### Python
```python
def reverseBits(n):
    # Swap bits in groups of 1
    n = ((n & 0xAAAAAAAA) >> 1) | ((n & 0x55555555) << 1)
    # Swap bits in groups of 2
    n = ((n & 0xCCCCCCCC) >> 2) | ((n & 0x33333333) << 2)
    # Swap bits in groups of 4
    n = ((n & 0xF0F0F0F0) >> 4) | ((n & 0x0F0F0F0F) << 4)
    # Swap bits in groups of 8
    n = ((n & 0xFF00FF00) >> 8) | ((n & 0x00FF00FF) << 8)
    # Swap bits in groups of 16
    n = ((n & 0xFFFF0000) >> 16) | ((n & 0x0000FFFF) << 16)
    return n
```

#### JavaScript
```javascript
function reverseBits(n) {
    // Swap bits in groups of 1
    n = ((n & 0xAAAAAAAA) >>> 1) | ((n & 0x55555555) << 1);
    // Swap bits in groups of 2
    n = ((n & 0xCCCCCCCC) >>> 2) | ((n & 0x33333333) << 2);
    // Swap bits in groups of 4
    n = ((n & 0xF0F0F0F0) >>> 4) | ((n & 0x0F0F0F0F) << 4);
    // Swap bits in groups of 8
    n = ((n & 0xFF00FF00) >>> 8) | ((n & 0x00FF00FF) << 8);
    // Swap bits in groups of 16
    n = ((n & 0xFFFF0000) >>> 16) | ((n & 0x0000FFFF) << 16);
    return n >>> 0;  // Force unsigned 32-bit integer
}
```

#### Java
```java
public int reverseBits(int n) {
    // Swap bits in groups of 1
    n = ((n & 0xAAAAAAAA) >>> 1) | ((n & 0x55555555) << 1);
    // Swap bits in groups of 2
    n = ((n & 0xCCCCCCCC) >>> 2) | ((n & 0x33333333) << 2);
    // Swap bits in groups of 4
    n = ((n & 0xF0F0F0F0) >>> 4) | ((n & 0x0F0F0F0F) << 4);
    // Swap bits in groups of 8
    n = ((n & 0xFF00FF00) >>> 8) | ((n & 0x00FF00FF) << 8);
    // Swap bits in groups of 16
    n = ((n & 0xFFFF0000) >>> 16) | ((n & 0x0000FFFF) << 16);
    return n;
}
```

### 4. Using Lookup Table (Fast for Multiple Queries)
- **Time complexity**: O(1)
- **Space complexity**: O(1) for the algorithm, O(256) for the lookup table

#### Python
```python
def reverseBits(n):
    # Initialize the lookup table for byte reversal (if needed)
    lookup_table = [0] * 256
    for i in range(256):
        lookup_table[i] = (i * 0x0202020202 & 0x010884422010) % 1023

    # Split the 32-bit integer into 4 bytes and reverse each byte using the lookup table
    result = 0
    result += lookup_table[n & 0xff] << 24
    result += lookup_table[(n >> 8) & 0xff] << 16
    result += lookup_table[(n >> 16) & 0xff] << 8
    result += lookup_table[(n >> 24) & 0xff]
    
    return result
```

#### JavaScript
```javascript
function reverseBits(n) {
    // Initialize the lookup table for byte reversal
    const lookupTable = new Array(256);
    for (let i = 0; i < 256; i++) {
        lookupTable[i] = (((i * 0x0202020202) & 0x010884422010) % 1023);
    }
    
    // Split the 32-bit integer into 4 bytes and reverse each byte using the lookup table
    let result = 0;
    result += (lookupTable[n & 0xff]) << 24;
    result += (lookupTable[(n >>> 8) & 0xff]) << 16;
    result += (lookupTable[(n >>> 16) & 0xff]) << 8;
    result += (lookupTable[(n >>> 24) & 0xff]);
    
    return result >>> 0;  // Force unsigned 32-bit integer
}
```

#### Java
```java
public int reverseBits(int n) {
    // Initialize the lookup table for byte reversal
    int[] lookupTable = new int[256];
    for (int i = 0; i < 256; i++) {
        lookupTable[i] = (int)((i * 0x0202020202L & 0x010884422010L) % 1023);
    }
    
    // Split the 32-bit integer into 4 bytes and reverse each byte using the lookup table
    int result = 0;
    result += (lookupTable[n & 0xff]) << 24;
    result += (lookupTable[(n >>> 8) & 0xff]) << 16;
    result += (lookupTable[(n >>> 16) & 0xff]) << 8;
    result += (lookupTable[(n >>> 24) & 0xff]);
    
    return result;
}
```
