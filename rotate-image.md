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

### 2. Rotate Four Cells at a Time
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
