# Rotate Image

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

## Approaches

### 1. Brute Force
- **Time complexity**: O(n²)
- **Space complexity**: O(n²)

#### Python
```python
def rotate(matrix):
    n = len(matrix)
    rotated = [[0] * n for _ in range(n)]
    
    for i in range(n):
        for j in range(n):
            rotated[j][n - 1 - i] = matrix[i][j]
    
    for i in range(n):
        for j in range(n):
            matrix[i][j] = rotated[i][j]
```

#### JavaScript
```javascript
function rotate(matrix) {
    const n = matrix.length;
    const rotated = Array(n).fill().map(() => Array(n).fill(0));
    
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            rotated[j][n - 1 - i] = matrix[i][j];
        }
    }
    
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            matrix[i][j] = rotated[i][j];
        }
    }
}
```

#### Java
```java
public void rotate(int[][] matrix) {
    int n = matrix.length;
    int[][] rotated = new int[n][n];
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            rotated[j][n - 1 - i] = matrix[i][j];
        }
    }
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            matrix[i][j] = rotated[i][j];
        }
    }
}
```

### 2. Rotate Four Cells at a Time ✅✅✅

- **Time complexity**: O(n²)
- **Space complexity**: O(1)

#### Python

```python
def rotate(self, matrix: List[List[int]]) -> None:
    l, r = 0, len(matrix) - 1
    
    while l < r:  # loop through layers of matrix
        for i in range(r - l):  # loop through elements in current layer
            top, bottom = l, r
            
            # save the topLeft
            topLeft = matrix[top][l + i]
            
            # move bottom-left to top-left
            matrix[top][l + i] = matrix[bottom - i][l]
            
            # move bottom-right to bottom-left
            matrix[bottom - i][l] = matrix[bottom][r - i]
            
            # move top-right to bottom-right
            matrix[bottom][r - i] = matrix[top + i][r]
            
            # set top-right with saved top-left
            matrix[top + i][r] = topLeft
            
        l += 1
        r -= 1
```
##### Matrix Rotation Algorithm - Clear Explanation

#### Original Matrix

```
+-----+-----+-----+-----+
|  1  |  2  |  3  |  4  |
+-----+-----+-----+-----+
|  5  |  6  |  7  |  8  |
+-----+-----+-----+-----+
|  9  | 10  | 11  | 12  |
+-----+-----+-----+-----+
| 13  | 14  | 15  | 16  |
+-----+-----+-----+-----+
```

#### Layer 1, Iteration 1

Elements being moved: 1, 4, 16, 13

```
+-----+-----+-----+-----+
|  1* |  2  |  3  |  4* |
+-----+-----+-----+-----+
|  5  |  6  |  7  |  8  |
+-----+-----+-----+-----+
|  9  | 10  | 11  | 12  |
+-----+-----+-----+-----+
| 13* | 14  | 15  | 16* |
+-----+-----+-----+-----+
```

After moving:
- Save 1 (temporary)
- Move 13 to position [0,0]
- Move 16 to position [3,0]
- Move 4 to position [3,3]
- Move saved 1 to position [0,3]

```
+-----+-----+-----+-----+
| 13  |  2  |  3  |  1  |
+-----+-----+-----+-----+
|  5  |  6  |  7  |  8  |
+-----+-----+-----+-----+
|  9  | 10  | 11  | 12  |
+-----+-----+-----+-----+
| 16  | 14  | 15  |  4  |
+-----+-----+-----+-----+
```

#### Layer 1, Iteration 2

Elements being moved: 2, 8, 15, 9

```
+-----+-----+-----+-----+
| 13  |  2* |  3  |  1  |
+-----+-----+-----+-----+
|  5  |  6  |  7  |  8* |
+-----+-----+-----+-----+
|  9* | 10  | 11  | 12  |
+-----+-----+-----+-----+
| 16  | 14  | 15* |  4  |
+-----+-----+-----+-----+
```

After moving:
- Save 2 (temporary)
- Move 9 to position [0,1]
- Move 15 to position [2,0]
- Move 8 to position [3,2]
- Move saved 2 to position [1,3]

```
+-----+-----+-----+-----+
| 13  |  9  |  3  |  1  |
+-----+-----+-----+-----+
|  5  |  6  |  7  |  2  |
+-----+-----+-----+-----+
| 15  | 10  | 11  | 12  |
+-----+-----+-----+-----+
| 16  | 14  |  8  |  4  |
+-----+-----+-----+-----+
```

#### Layer 1, Iteration 3

Elements being moved: 3, 12, 14, 5

```
+-----+-----+-----+-----+
| 13  |  9  |  3* |  1  |
+-----+-----+-----+-----+
|  5* |  6  |  7  |  2  |
+-----+-----+-----+-----+
| 15  | 10  | 11  | 12* |
+-----+-----+-----+-----+
| 16  | 14* |  8  |  4  |
+-----+-----+-----+-----+
```

After moving:
- Save 3 (temporary)
- Move 5 to position [0,2]
- Move 14 to position [1,0]
- Move 12 to position [3,1]
- Move saved 3 to position [2,3]

```
+-----+-----+-----+-----+
| 13  |  9  |  5  |  1  |
+-----+-----+-----+-----+
| 14  |  6  |  7  |  2  |
+-----+-----+-----+-----+
| 15  | 10  | 11  |  3  |
+-----+-----+-----+-----+
| 16  | 12  |  8  |  4  |
+-----+-----+-----+-----+
```

#### Layer 2, Iteration 1

Elements being moved: 6, 7, 11, 10

```
+-----+-----+-----+-----+
| 13  |  9  |  5  |  1  |
+-----+-----+-----+-----+
| 14  |  6* |  7* |  2  |
+-----+-----+-----+-----+
| 15  | 10* | 11* |  3  |
+-----+-----+-----+-----+
| 16  | 12  |  8  |  4  |
+-----+-----+-----+-----+
```

After moving:
- Save 6 (temporary)
- Move 10 to position [1,1]
- Move 11 to position [2,1]
- Move 7 to position [2,2]
- Move saved 6 to position [1,2]

```
+-----+-----+-----+-----+
| 13  |  9  |  5  |  1  |
+-----+-----+-----+-----+
| 14  | 10  |  6  |  2  |
+-----+-----+-----+-----+
| 15  | 11  |  7  |  3  |
+-----+-----+-----+-----+
| 16  | 12  |  8  |  4  |
+-----+-----+-----+-----+
```

#### Final Rotated Matrix

The matrix after all rotations:

```
+-----+-----+-----+-----+
| 13  |  9  |  5  |  1  |
+-----+-----+-----+-----+
| 14  | 10  |  6  |  2  |
+-----+-----+-----+-----+
| 15  | 11  |  7  |  3  |
+-----+-----+-----+-----+
| 16  | 12  |  8  |  4  |
+-----+-----+-----+-----+
```

Compare with the original matrix rotated 90° clockwise:

```
Original:                Rotated 90° Clockwise:
+-----+-----+-----+-----+  +-----+-----+-----+-----+
|  1  |  2  |  3  |  4  |  | 13  |  9  |  5  |  1  |
+-----+-----+-----+-----+  +-----+-----+-----+-----+
|  5  |  6  |  7  |  8  |  | 14  | 10  |  6  |  2  |
+-----+-----+-----+-----+  +-----+-----+-----+-----+
|  9  | 10  | 11  | 12  |  | 15  | 11  |  7  |  3  |
+-----+-----+-----+-----+  +-----+-----+-----+-----+
| 13  | 14  | 15  | 16  |  | 16  | 12  |  8  |  4  |
+-----+-----+-----+-----+  +-----+-----+-----+-----+
```

The algorithm correctly rotates the matrix 90° clockwise.

#### JavaScript

```javascript
/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
function rotate(matrix) {
    let l = 0;
    let r = matrix.length - 1;
    
    while (l < r) {  // loop through layers of matrix
        for (let i = 0; i < r - l; i++) {  // loop through elements in current layer
            let top = l;
            let bottom = r;
            
            // save the topLeft
            let topLeft = matrix[top][l + i];
            
            // move bottom-left to top-left
            matrix[top][l + i] = matrix[bottom - i][l];
            
            // move bottom-right to bottom-left
            matrix[bottom - i][l] = matrix[bottom][r - i];
            
            // move top-right to bottom-right
            matrix[bottom][r - i] = matrix[top + i][r];
            
            // set top-right with saved top-left
            matrix[top + i][r] = topLeft;
        }
        
        l += 1;
        r -= 1;
    }
}
```

#### Java

```java
public void rotate(int[][] matrix) {
    int l = 0;
    int r = matrix.length - 1;
    
    while (l < r) {  // loop through layers of matrix
        for (int i = 0; i < r - l; i++) {  // loop through elements in current layer
            int top = l;
            int bottom = r;
            
            // save the topLeft
            int topLeft = matrix[top][l + i];
            
            // move bottom-left to top-left
            matrix[top][l + i] = matrix[bottom - i][l];
            
            // move bottom-right to bottom-left
            matrix[bottom - i][l] = matrix[bottom][r - i];
            
            // move top-right to bottom-right
            matrix[bottom][r - i] = matrix[top + i][r];
            
            // set top-right with saved top-left
            matrix[top + i][r] = topLeft;
        }
        
        l++;
        r--;
    }
}
```

### 3. Transpose + Reverse
- **Time complexity**: O(n²)
- **Space complexity**: O(1)

#### Python
```python
def rotate(matrix):
    n = len(matrix)
    
    # Transpose the matrix
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Reverse each row
    for i in range(n):
        for j in range(n // 2):
            matrix[i][j], matrix[i][n - j - 1] = matrix[i][n - j - 1], matrix[i][j]
```

#### JavaScript
```javascript
function rotate(matrix) {
    const n = matrix.length;
    
    // Transpose the matrix
    for (let i = 0; i < n; i++) {
        for (let j = i; j < n; j++) {
            [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
        }
    }
    
    // Reverse each row
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < Math.floor(n / 2); j++) {
            [matrix[i][j], matrix[i][n - j - 1]] = [matrix[i][n - j - 1], matrix[i][j]];
        }
    }
}
```

#### Java
```java
public void rotate(int[][] matrix) {
    int n = matrix.length;
    
    // Transpose the matrix
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
    
    // Reverse each row
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n / 2; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[i][n - j - 1];
            matrix[i][n - j - 1] = temp;
        }
    }
}
```

### 4. Reverse + Transpose
- **Time complexity**: O(n²)
- **Space complexity**: O(1)

#### Python
```python
def rotate(matrix):
    # Reverse the matrix vertically
    matrix.reverse()
    
    # Transpose the matrix
    for i in range(len(matrix)):
        for j in range(i + 1, len(matrix)):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```

#### JavaScript
```javascript
function rotate(matrix) {
    // Reverse the matrix vertically
    matrix.reverse();
    
    // Transpose the matrix
    for (let i = 0; i < matrix.length; i++) {
        for (let j = i + 1; j < matrix.length; j++) {
            [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
        }
    }
}
```

#### Java
```java
public void rotate(int[][] matrix) {
    // Reverse the matrix vertically
    int n = matrix.length;
    for (int i = 0; i < n / 2; i++) {
        int[] temp = matrix[i];
        matrix[i] = matrix[n - 1 - i];
        matrix[n - 1 - i] = temp;
    }
    
    // Transpose the matrix
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
}
```
