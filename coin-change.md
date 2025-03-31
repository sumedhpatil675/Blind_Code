# Coin Change

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

## Approaches

### 1. Recursion

**Time complexity:** O(n^t)  
**Space complexity:** O(t)  
where n is the length of the array coins and t is the given amount.

#### Python
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        def dfs(amount):
            if amount == 0:
                return 0
                
            res = float('inf')
            
            for coin in coins:
                if amount - coin >= 0:
                    res = min(res, 1 + dfs(amount - coin))
                    
            return res
            
        minCoins = dfs(amount)
        return -1 if minCoins == float('inf') else minCoins
```

#### JavaScript
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    function dfs(amount) {
        if (amount === 0) {
            return 0;
        }
        
        let res = Infinity;
        
        for (const coin of coins) {
            if (amount - coin >= 0) {
                res = Math.min(res, 1 + dfs(amount - coin));
            }
        }
        
        return res;
    }
    
    const minCoins = dfs(amount);
    return minCoins === Infinity ? -1 : minCoins;
};
```

#### Java
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int result = dfs(coins, amount);
        return result == Integer.MAX_VALUE ? -1 : result;
    }
    
    private int dfs(int[] coins, int amount) {
        if (amount == 0) {
            return 0;
        }
        
        int res = Integer.MAX_VALUE;
        
        for (int coin : coins) {
            if (amount - coin >= 0) {
                int subResult = dfs(coins, amount - coin);
                if (subResult != Integer.MAX_VALUE) {
                    res = Math.min(res, 1 + subResult);
                }
            }
        }
        
        return res;
    }
}
```

### 2. Dynamic Programming (Top-Down)

**Time complexity:** O(n*t)  
**Space complexity:** O(t)  
where n is the length of the array coins and t is the given amount.

#### Python
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        memo = {}
        
        def dfs(amount):
            if amount == 0:
                return 0
                
            if amount in memo:
                return memo[amount]
                
            res = float('inf')
            
            for coin in coins:
                if amount - coin >= 0:
                    res = min(res, 1 + dfs(amount - coin))
                    
            memo[amount] = res
            return res
            
        minCoins = dfs(amount)
        return -1 if minCoins == float('inf') else minCoins
```

#### JavaScript
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    const memo = new Map();
    
    function dfs(amount) {
        if (amount === 0) {
            return 0;
        }
        
        if (memo.has(amount)) {
            return memo.get(amount);
        }
        
        let res = Infinity;
        
        for (const coin of coins) {
            if (amount - coin >= 0) {
                res = Math.min(res, 1 + dfs(amount - coin));
            }
        }
        
        memo.set(amount, res);
        return res;
    }
    
    const minCoins = dfs(amount);
    return minCoins === Infinity ? -1 : minCoins;
};
```

#### Java
```java
class Solution {
    private Map<Integer, Integer> memo = new HashMap<>();
    
    public int coinChange(int[] coins, int amount) {
        int result = dfs(coins, amount);
        return result == Integer.MAX_VALUE ? -1 : result;
    }
    
    private int dfs(int[] coins, int amount) {
        if (amount == 0) {
            return 0;
        }
        
        if (memo.containsKey(amount)) {
            return memo.get(amount);
        }
        
        int res = Integer.MAX_VALUE;
        
        for (int coin : coins) {
            if (amount - coin >= 0) {
                int subResult = dfs(coins, amount - coin);
                if (subResult != Integer.MAX_VALUE) {
                    res = Math.min(res, 1 + subResult);
                }
            }
        }
        
        memo.put(amount, res);
        return res;
    }
}
```

### 3. Dynamic Programming (Bottom-Up)

**Time complexity:** O(n*t)  
**Space complexity:** O(t)  
where n is the length of the array coins and t is the given amount.

#### Python
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [amount + 1] * (amount + 1)
        dp[0] = 0
        
        for a in range(1, amount + 1):
            for c in coins:
                if a - c >= 0:
                    dp[a] = min(dp[a], 1 + dp[a - c])
                    
        return dp[amount] if dp[amount] != amount + 1 else -1
```

#### JavaScript
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    const dp = Array(amount + 1).fill(amount + 1);
    dp[0] = 0;
    
    for (let a = 1; a <= amount; a++) {
        for (const coin of coins) {
            if (a - coin >= 0) {
                dp[a] = Math.min(dp[a], 1 + dp[a - coin]);
            }
        }
    }
    
    return dp[amount] === amount + 1 ? -1 : dp[amount];
};
```

#### Java
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        
        for (int a = 1; a <= amount; a++) {
            for (int coin : coins) {
                if (a - coin >= 0) {
                    dp[a] = Math.min(dp[a], 1 + dp[a - coin]);
                }
            }
        }
        
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

### 4. Breadth First Search

**Time complexity:** O(n*t)  
**Space complexity:** O(t)  
where n is the length of the array coins and t is the given amount.

#### Python
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        if amount == 0:
            return 0
            
        q = deque([0])
        seen = [False] * (amount + 1)
        seen[0] = True
        res = 0
        
        while q:
            res += 1
            
            for _ in range(len(q)):
                cur = q.popleft()
                
                for coin in coins:
                    nxt = cur + coin
                    
                    if nxt == amount:
                        return res
                        
                    if nxt > amount or seen[nxt]:
                        continue
                        
                    seen[nxt] = True
                    q.append(nxt)
                    
        return -1
```

#### JavaScript
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    if (amount === 0) {
        return 0;
    }
    
    const queue = [0];
    const seen = new Array(amount + 1).fill(false);
    seen[0] = true;
    let level = 0;
    
    while (queue.length > 0) {
        level++;
        const size = queue.length;
        
        for (let i = 0; i < size; i++) {
            const curr = queue.shift();
            
            for (const coin of coins) {
                const next = curr + coin;
                
                if (next === amount) {
                    return level;
                }
                
                if (next > amount || seen[next]) {
                    continue;
                }
                
                seen[next] = true;
                queue.push(next);
            }
        }
    }
    
    return -1;
};
```

#### Java
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount == 0) {
            return 0;
        }
        
        Queue<Integer> queue = new LinkedList<>();
        boolean[] seen = new boolean[amount + 1];
        queue.offer(0);
        seen[0] = true;
        int level = 0;
        
        while (!queue.isEmpty()) {
            level++;
            int size = queue.size();
            
            for (int i = 0; i < size; i++) {
                int curr = queue.poll();
                
                for (int coin : coins) {
                    int next = curr + coin;
                    
                    if (next == amount) {
                        return level;
                    }
                    
                    if (next > amount || seen[next]) {
                        continue;
                    }
                    
                    seen[next] = true;
                    queue.offer(next);
                }
            }
        }
        
        return -1;
    }
}
```