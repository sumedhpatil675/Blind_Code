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
def rotate(matrix):
    n = len(matrix)
    for layer in range(n // 2):
        first, last = layer, n - 1 - layer
        for i in range(first, last):
            offset = i - first
            
            # Save top
            top = matrix[first][i]
            
            # Left -> Top
            matrix[first][i] = matrix[last - offset][first]
            
            # Bottom -> Left
            matrix[last - offset][first] = matrix[last][last - offset]
            
            # Right -> Bottom
            matrix[last][last - offset] = matrix[i][last]
            
            # Top -> Right
            matrix[i][last] = top
```

#### JavaScript
```javascript
function rotate(matrix) {
    const n = matrix.length;
    for (let layer = 0; layer < Math.floor(n / 2); layer++) {
        const first = layer;
        const last = n - 1 - layer;
        for (let i = first; i < last; i++) {
            const offset = i - first;
            
            // Save top
            const top = matrix[first][i];
            
            // Left -> Top
            matrix[first][i] = matrix[last - offset][first];
            
            // Bottom -> Left
            matrix[last - offset][first] = matrix[last][last - offset];
            
            // Right -> Bottom
            matrix[last][last - offset] = matrix[i][last];
            
            // Top -> Right
            matrix[i][last] = top;
        }
    }
}
```

#### Java
```java
public void rotate(int[][] matrix) {
    int n = matrix.length;
    for (int layer = 0; layer < n / 2; layer++) {
        int first = layer;
        int last = n - 1 - layer;
        for (int i = first; i < last; i++) {
            int offset = i - first;
            
            // Save top
            int top = matrix[first][i];
            
            // Left -> Top
            matrix[first][i] = matrix[last - offset][first];
            
            // Bottom -> Left
            matrix[last - offset][first] = matrix[last][last - offset];
            
            // Right -> Bottom
            matrix[last][last - offset] = matrix[i][last];
            
            // Top -> Right
            matrix[i][last] = top;
        }
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