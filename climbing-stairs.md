# Climbing Stairs

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

## Approaches

### 1. Recursion

**Time complexity:** O(2^n)  
**Space complexity:** O(n)

#### Python

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        def dfs(i):
            if i >= n:
                return i == n

            return dfs(i + 1) + dfs(i + 2)

        return dfs(0)
```

#### JavaScript

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
  function dfs(i) {
    if (i >= n) {
      return i === n ? 1 : 0;
    }

    return dfs(i + 1) + dfs(i + 2);
  }

  return dfs(0);
};
```

#### Java

```java
class Solution {
    public int climbStairs(int n) {
        return dfs(0, n);
    }

    private int dfs(int i, int n) {
        if (i > n) {
            return 0;
        }
        if (i == n) {
            return 1;
        }

        return dfs(i + 1, n) + dfs(i + 2, n);
    }
}
```

### 2. Dynamic Programming (Top-Down)

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        cache = [-1] * n

        def dfs(i):
            if i >= n:
                return i == n

            if cache[i] != -1:
                return cache[i]

            cache[i] = dfs(i + 1) + dfs(i + 2)
            return cache[i]

        return dfs(0)
```

#### JavaScript

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
  const cache = Array(n).fill(-1);

  function dfs(i) {
    if (i >= n) {
      return i === n ? 1 : 0;
    }

    if (cache[i] !== -1) {
      return cache[i];
    }

    cache[i] = dfs(i + 1) + dfs(i + 2);
    return cache[i];
  }

  return dfs(0);
};
```

#### Java

```java
class Solution {
    public int climbStairs(int n) {
        int[] cache = new int[n + 1];
        Arrays.fill(cache, -1);
        return dfs(0, n, cache);
    }

    private int dfs(int i, int n, int[] cache) {
        if (i > n) {
            return 0;
        }
        if (i == n) {
            return 1;
        }

        if (cache[i] != -1) {
            return cache[i];
        }

        cache[i] = dfs(i + 1, n, cache) + dfs(i + 2, n, cache);
        return cache[i];
    }
}
```

### 3. Dynamic Programming (Bottom-Up)

**Time complexity:** O(n)  
**Space complexity:** O(n)

#### Python

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n

        dp = [0] * (n + 1)
        dp[1], dp[2] = 1, 2

        for i in range(3, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]

        return dp[n]
```

#### JavaScript

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
  if (n <= 2) {
    return n;
  }

  const dp = Array(n + 1).fill(0);
  dp[1] = 1;
  dp[2] = 2;

  for (let i = 3; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }

  return dp[n];
};
```

#### Java

```java
class Solution {
    public int climbStairs(int n) {
        if (n <= 2) {
            return n;
        }

        int[] dp = new int[n + 1];
        dp[1] = 1;
        dp[2] = 2;

        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
    }
}
```

### 4. Dynamic Programming (Space Optimized)

**Time complexity:** O(n)  
**Space complexity:** O(1)

#### Python

```python
def climbStairs(n: int) -> int:
    previous_step, current_step = 1, 1
    for i in range(n - 1):
        # Update the steps by moving the current step to the previous and calculating the new current step
        next_step = previous_step + current_step
        previous_step = current_step
        current_step = next_step
    return current_step
```

#### JavaScript

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
  let previous_step = 1;
  let current_step = 1;

  for (let i = 0; i < n - 1; i++) {
    // Update the steps by moving the current step to the previous and calculating the new current step
    const next_step = previous_step + current_step;
    previous_step = current_step;
    current_step = next_step;
  }

  return current_step;
};
```

#### Java

```java
class Solution {
    public int climbStairs(int n) {
        int previous_step = 1;
        int current_step = 1;

        for (int i = 0; i < n - 1; i++) {
            // Update the steps by moving the current step to the previous and calculating the new current step
            int next_step = previous_step + current_step;
            previous_step = current_step;
            current_step = next_step;
        }

        return current_step;
    }
}
```

### 5. Matrix Exponentiation

**Time complexity:** O(log n)  
**Space complexity:** O(1)

#### Python

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1

        def matrix_mult(A, B):
            return [
                [A[0][0] * B[0][0] + A[0][1] * B[1][0], A[0][0] * B[0][1] + A[0][1] * B[1][1]],
                [A[1][0] * B[0][0] + A[1][1] * B[1][0], A[1][0] * B[0][1] + A[1][1] * B[1][1]]
            ]

        def matrix_pow(M, p):
            result = [[1, 0], [0, 1]]
            base = M

            while p:
                if p % 2 == 1:
                    result = matrix_mult(result, base)
                base = matrix_mult(base, base)
                p //= 2

            return result

        M = [[1, 1], [1, 0]]
        result = matrix_pow(M, n)
        return result[0][0]
```

#### JavaScript

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
  if (n === 1) {
    return 1;
  }

  function matrixMultiply(A, B) {
    return [
      [
        A[0][0] * B[0][0] + A[0][1] * B[1][0],
        A[0][0] * B[0][1] + A[0][1] * B[1][1],
      ],
      [
        A[1][0] * B[0][0] + A[1][1] * B[1][0],
        A[1][0] * B[0][1] + A[1][1] * B[1][1],
      ],
    ];
  }

  function matrixPower(M, p) {
    let result = [
      [1, 0],
      [0, 1],
    ]; // Identity matrix
    let base = M;

    while (p > 0) {
      if (p % 2 === 1) {
        result = matrixMultiply(result, base);
      }
      base = matrixMultiply(base, base);
      p = Math.floor(p / 2);
    }

    return result;
  }

  const M = [
    [1, 1],
    [1, 0],
  ];
  const result = matrixPower(M, n);
  return result[0][0];
};
```

#### Java

```java
class Solution {
    public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }

        int[][] M = {{1, 1}, {1, 0}};
        int[][] result = matrixPower(M, n);

        return result[0][0];
    }

    private int[][] matrixMultiply(int[][] A, int[][] B) {
        int[][] C = new int[2][2];
        C[0][0] = A[0][0] * B[0][0] + A[0][1] * B[1][0];
        C[0][1] = A[0][0] * B[0][1] + A[0][1] * B[1][1];
        C[1][0] = A[1][0] * B[0][0] + A[1][1] * B[1][0];
        C[1][1] = A[1][0] * B[0][1] + A[1][1] * B[1][1];
        return C;
    }

    private int[][] matrixPower(int[][] M, int n) {
        int[][] result = {{1, 0}, {0, 1}}; // Identity matrix

        while (n > 0) {
            if (n % 2 == 1) {
                result = matrixMultiply(result, M);
            }
            M = matrixMultiply(M, M);
            n /= 2;
        }

        return result;
    }
}
```

### 6. Math

**Time complexity:** O(log n)  
**Space complexity:** O(1)

#### Python

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        sqrt5 = math.sqrt(5)
        phi = (1 + sqrt5) / 2
        psi = (1 - sqrt5) / 2

        n += 1
        return round((phi**n - psi**n) / sqrt5)
```

#### JavaScript

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
  const sqrt5 = Math.sqrt(5);
  const phi = (1 + sqrt5) / 2;
  const psi = (1 - sqrt5) / 2;

  n += 1;
  return Math.round((Math.pow(phi, n) - Math.pow(psi, n)) / sqrt5);
};
```

#### Java

```java
class Solution {
    public int climbStairs(int n) {
        double sqrt5 = Math.sqrt(5);
        double phi = (1 + sqrt5) / 2;
        double psi = (1 - sqrt5) / 2;

        n += 1;
        return (int) Math.round((Math.pow(phi, n) - Math.pow(psi, n)) / sqrt5);
    }
}
```
