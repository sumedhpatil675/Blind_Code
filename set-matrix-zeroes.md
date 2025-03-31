# Set Matrix Zeroes

Given an m x n integer matrix matrix, if an element is 0, set its entire row and column to 0's. You must do it in place.

## Approaches

### 1. Brute Force
- **Time complexity**: O(m*n*(m+n))
- **Space complexity**: O(1)

#### Python
```python
def setZeroes(matrix):
    ROWS, COLS = len(matrix), len(matrix[0])
    
    def markRow(r):
        for c in range(COLS):
            if matrix[r][c] != 0:
                matrix[r][c] = float('inf')
    
    def markCol(c):
        for r in range(ROWS):
            if matrix[r][c] != 0:
                matrix[r][c] = float('inf')
    
    # Mark rows and columns with infinity
    for r in range(ROWS):
        for c in range(COLS):
            if matrix[r][c] == 0:
                markRow(r)
                markCol(c)
    
    # Convert infinity to zero
    for r in range(ROWS):
        for c in range(COLS):
            if matrix[r][c] == float('inf'):
                matrix[r][c] = 0
```

#### JavaScript
```javascript
function setZeroes(matrix) {
    const ROWS = matrix.length;
    const COLS = matrix[0].length;
    
    function markRow(r) {
        for (let c = 0; c < COLS; c++) {
            if (matrix[r][c] !== 0) {
                matrix[r][c] = Infinity;
            }
        }
    }
    
    function markCol(c) {
        for (let r = 0; r < ROWS; r++) {
            if (matrix[r][c] !== 0) {
                matrix[r][c] = Infinity;
            }
        }
    }
    
    // Mark rows and columns with infinity
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (matrix[r][c] === 0) {
                markRow(r);
                markCol(c);
            }
        }
    }
    
    // Convert infinity to zero
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (matrix[r][c] === Infinity) {
                matrix[r][c] = 0;
            }
        }
    }
}
```

#### Java
```java
public void setZeroes(int[][] matrix) {
    int ROWS = matrix.length;
    int COLS = matrix[0].length;
    
    for (int r = 0; r < ROWS; r++) {
        for (int c = 0; c < COLS; c++) {
            if (matrix[r][c] == 0) {
                // Mark rows and columns
                for (int i = 0; i < COLS; i++) {
                    if (matrix[r][i] != 0) {
                        matrix[r][i] = Integer.MIN_VALUE;
                    }
                }
                for (int i = 0; i < ROWS; i++) {
                    if (matrix[i][c] != 0) {
                        matrix[i][c] = Integer.MIN_VALUE;
                    }
                }
            }
        }
    }
    
    // Convert marked values to zero
    for (int r = 0; r < ROWS; r++) {
        for (int c = 0; c < COLS; c++) {
            if (matrix[r][c] == Integer.MIN_VALUE) {
                matrix[r][c] = 0;
            }
        }
    }
}
```

### 2. Using Additional Arrays
- **Time complexity**: O(m*n)
- **Space complexity**: O(m+n)

#### Python
```python
def setZeroes(matrix):
    ROWS, COLS = len(matrix), len(matrix[0])
    rows, cols = [False] * ROWS, [False] * COLS
    
    # Mark rows and columns to be zeroed
    for r in range(ROWS):
        for c in range(COLS):
            if matrix[r][c] == 0:
                rows[r] = True
                cols[c] = True
    
    # Set zeros
    for r in range(ROWS):
        for c in range(COLS):
            if rows[r] or cols[c]:
                matrix[r][c] = 0
```

#### JavaScript
```javascript
function setZeroes(matrix) {
    const ROWS = matrix.length;
    const COLS = matrix[0].length;
    const rows = Array(ROWS).fill(false);
    const cols = Array(COLS).fill(false);
    
    // Mark rows and columns to be zeroed
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (matrix[r][c] === 0) {
                rows[r] = true;
                cols[c] = true;
            }
        }
    }
    
    // Set zeros
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (rows[r] || cols[c]) {
                matrix[r][c] = 0;
            }
        }
    }
}
```

#### Java
```java
public void setZeroes(int[][] matrix) {
    int ROWS = matrix.length;
    int COLS = matrix[0].length;
    boolean[] rows = new boolean[ROWS];
    boolean[] cols = new boolean[COLS];
    
    // Mark rows and columns to be zeroed
    for (int r = 0; r < ROWS; r++) {
        for (int c = 0; c < COLS; c++) {
            if (matrix[r][c] == 0) {
                rows[r] = true;
                cols[c] = true;
            }
        }
    }
    
    // Set zeros
    for (int r = 0; r < ROWS; r++) {
        for (int c = 0; c < COLS; c++) {
            if (rows[r] || cols[c]) {
                matrix[r][c] = 0;
            }
        }
    }
}
```

### 3. Using First Row and Column as Markers
- **Time complexity**: O(m*n)
- **Space complexity**: O(1)

#### Python
```python
def setZeroes(matrix):
    ROWS, COLS = len(matrix), len(matrix[0])
    rowZero = False
    
    # Determine if first row should be zeroed
    for c in range(COLS):
        if matrix[0][c] == 0:
            rowZero = True
            break
    
    # Use first row and first column as markers
    for r in range(1, ROWS):
        for c in range(COLS):
            if matrix[r][c] == 0:
                matrix[0][c] = 0
                matrix[r][0] = 0
    
    # Set zeros based on markers (except first row and first column)
    for r in range(1, ROWS):
        for c in range(1, COLS):
            if matrix[0][c] == 0 or matrix[r][0] == 0:
                matrix[r][c] = 0
    
    # Set first column to zeros if needed
    if matrix[0][0] == 0:
        for r in range(ROWS):
            matrix[r][0] = 0
    
    # Set first row to zeros if needed
    if rowZero:
        for c in range(COLS):
            matrix[0][c] = 0
```

#### JavaScript
```javascript
function setZeroes(matrix) {
    const ROWS = matrix.length;
    const COLS = matrix[0].length;
    let rowZero = false;
    
    // Determine if first row should be zeroed
    for (let c = 0; c < COLS; c++) {
        if (matrix[0][c] === 0) {
            rowZero = true;
            break;
        }
    }
    
    // Use first row and first column as markers
    for (let r = 1; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (matrix[r][c] === 0) {
                matrix[0][c] = 0;
                matrix[r][0] = 0;
            }
        }
    }
    
    // Set zeros based on markers (except first row and first column)
    for (let r = 1; r < ROWS; r++) {
        for (let c = 1; c < COLS; c++) {
            if (matrix[0][c] === 0 || matrix[r][0] === 0) {
                matrix[r][c] = 0;
            }
        }
    }
    
    // Set first column to zeros if needed
    if (matrix[0][0] === 0) {
        for (let r = 0; r < ROWS; r++) {
            matrix[r][0] = 0;
        }
    }
    
    // Set first row to zeros if needed
    if (rowZero) {
        for (let c = 0; c < COLS; c++) {
            matrix[0][c] = 0;
        }
    }
}
```

#### Java
```java
public void setZeroes(int[][] matrix) {
    int ROWS = matrix.length;
    int COLS = matrix[0].length;
    boolean rowZero = false;
    
    // Determine if first row should be zeroed
    for (int c = 0; c < COLS; c++) {
        if (matrix[0][c] == 0) {
            rowZero = true;
            break;
        }
    }
    
    // Use first row and first column as markers
    for (int r = 1; r < ROWS; r++) {
        for (int c = 0; c < COLS; c++) {
            if (matrix[r][c] == 0) {
                matrix[0][c] = 0;
                matrix[r][0] = 0;
            }
        }
    }
    
    // Set zeros based on markers (except first row and first column)
    for (int r = 1; r < ROWS; r++) {
        for (int c = 1; c < COLS; c++) {
            if (matrix[0][c] == 0 || matrix[r][0] == 0) {
                matrix[r][c] = 0;
            }
        }
    }
    
    // Set first column to zeros if needed
    if (matrix[0][0] == 0) {
        for (int r = 0; r < ROWS; r++) {
            matrix[r][0] = 0;
        }
    }
    
    // Set first row to zeros if needed
    if (rowZero) {
        for (int c = 0; c < COLS; c++) {
            matrix[0][c] = 0;
        }
    }
}
```

### 4. Optimized In-place Solution
- **Time complexity**: O(m*n)
- **Space complexity**: O(1)

#### Python
```python
def setZeroes(matrix):
    ROWS, COLS = len(matrix), len(matrix[0])
    firstRowZero = False
    
    # Check if first row has any zeros
    for c in range(COLS):
        if matrix[0][c] == 0:
            firstRowZero = True
            break
    
    # Use first row as marker for columns
    # and first column as marker for rows
    for r in range(1, ROWS):
        for c in range(COLS):
            if matrix[r][c] == 0:
                matrix[r][0] = 0
                matrix[0][c] = 0
    
    # Set zeros based on first column markers (except first row)
    for r in range(1, ROWS):
        if matrix[r][0] == 0:
            for c in range(COLS):
                matrix[r][c] = 0
    
    # Set zeros based on first row markers
    for c in range(COLS):
        if matrix[0][c] == 0:
            for r in range(1, ROWS):
                matrix[r][c] = 0
    
    # Set first row to zeros if needed
    if firstRowZero:
        for c in range(COLS):
            matrix[0][c] = 0
```

#### JavaScript
```javascript
function setZeroes(matrix) {
    const ROWS = matrix.length;
    const COLS = matrix[0].length;
    let firstRowZero = false;
    
    // Check if first row has any zeros
    for (let c = 0; c < COLS; c++) {
        if (matrix[0][c] === 0) {
            firstRowZero = true;
            break;
        }
    }
    
    // Use first row as marker for columns
    // and first column as marker for rows
    for (let r = 1; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (matrix[r][c] === 0) {
                matrix[r][0] = 0;
                matrix[0][c] = 0;
            }
        }
    }
    
    // Set zeros based on first column markers (except first row)
    for (let r = 1; r < ROWS; r++) {
        if (matrix[r][0] === 0) {
            for (let c = 0; c < COLS; c++) {
                matrix[r][c] = 0;
            }
        }
    }
    
    // Set zeros based on first row markers
    for (let c = 0; c < COLS; c++) {
        if (matrix[0][c] === 0) {
            for (let r = 1; r < ROWS; r++) {
                matrix[r][c] = 0;
            }
        }
    }
    
    // Set first row to zeros if needed
    if (firstRowZero) {
        for (let c = 0; c < COLS; c++) {
            matrix[0][c] = 0;
        }
    }
}
```

#### Java
```java
public void setZeroes(int[][] matrix) {
    int ROWS = matrix.length;
    int COLS = matrix[0].length;
    boolean firstRowZero = false;
    
    // Check if first row has any zeros
    for (int c = 0; c < COLS; c++) {
        if (matrix[0][c] == 0) {
            firstRowZero = true;
            break;
        }
    }
    
    // Use first row as marker for columns
    // and first column as marker for rows
    for (int r = 1; r < ROWS; r++) {
        for (int c = 0; c < COLS; c++) {
            if (matrix[r][c] == 0) {
                matrix[r][0] = 0;
                matrix[0][c] = 0;
            }
        }
    }
    
    // Set zeros based on first column markers (except first row)
    for (int r = 1; r < ROWS; r++) {
        if (matrix[r][0] == 0) {
            for (int c = 0; c < COLS; c++) {
                matrix[r][c] = 0;
            }
        }
    }
    
    // Set zeros based on first row markers
    for (int c = 0; c < COLS; c++) {
        if (matrix[0][c] == 0) {
            for (int r = 1; r < ROWS; r++) {
                matrix[r][c] = 0;
            }
        }
    }
    
    // Set first row to zeros if needed
    if (firstRowZero) {
        for (int c = 0; c < COLS; c++) {
            matrix[0][c] = 0;
        }
    }
}
```