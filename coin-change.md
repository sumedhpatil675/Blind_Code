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

### 3. Dynamic Programming (Bottom-Up) ✅



### Using 2-D DP

## Time and Space Complexity
- **Time Complexity**: O(n × amount), where n is the number of coins
  - This is because we have nested loops: one iterating through all coin types (n) and the other through all amounts (amount)
    
- **Space Complexity**: O(n × amount)
  - We use a 2D array of size (n+1) × (amount+1)

## Python
```python
def coinChange(self, coins: List[int], amount: int) -> int:
    n = len(coins)
    maxVal = float("inf")
    res = [[0]*(amount+1)for _ in range(n+1)]
    for i in range(n+1):
        for j in range(amount+1):   ## (i->no of coins,j->amount)
            if i==0:  ##dont have any coins,to make required amount,hence set to infinity
                res[i][j] =  maxVal
            if j==0: ## means target amount is 0, hence required 0 min no of coins
                res[i][j] = 0
            if i==1: ## it means, we have only one coin, in array
                if(j%coins[0]==0): ## checking able to make required amount,with that coin
                    res[i][j] = j//coins[0] ## no of coins,required to make that amount
                else:
                    res[i][j] = maxVal ##not able to make that amount, set to infinity
    for i in range(2,n+1):
        for j in range(1,amount+1):
            if(coins[i-1]<=j): ## if curr coin value of array,is less than amount, then it means we can either add that coin(by doing +1 and subtracting that coin value from amount), or can neglect that coin, wil take minimum of this
                res[i][j] = min(1+res[i][j-coins[i-1]],res[i-1][j])
            else: ## coin value is > hence, will wil not add that coin
                res[i][j] = res[i-1][j]
    if(res[n][amount]==maxVal):
        return -1
    else:
        return(res[n][amount])
```

## JavaScript
```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
function coinChange(coins, amount) {
    const n = coins.length;
    const maxVal = Infinity;
    const res = Array(n+1).fill().map(() => Array(amount+1).fill(0));
    
    for (let i = 0; i <= n; i++) {
        for (let j = 0; j <= amount; j++) {  // (i->no of coins,j->amount)
            if (i === 0) { //dont have any coins,to make required amount,hence set to infinity
                res[i][j] = maxVal;
            }
            if (j === 0) { // means target amount is 0, hence required 0 min no of coins
                res[i][j] = 0;
            }
            if (i === 1) { // it means, we have only one coin, in array
                if (j % coins[0] === 0) { // checking able to make required amount,with that coin
                    res[i][j] = Math.floor(j / coins[0]); // no of coins,required to make that amount
                } else {
                    res[i][j] = maxVal; //not able to make that amount, set to infinity
                }
            }
        }
    }
    
    for (let i = 2; i <= n; i++) {
        for (let j = 1; j <= amount; j++) {
            if (coins[i-1] <= j) { // if curr coin value of array,is less than amount, then it means we can either add that coin(by doing +1 and subtracting that coin value from amount), or can neglect that coin, wil take minimum of this
                res[i][j] = Math.min(1 + res[i][j - coins[i-1]], res[i-1][j]);
            } else { // coin value is > hence, will wil not add that coin
                res[i][j] = res[i-1][j];
            }
        }
    }
    
    if (res[n][amount] === maxVal) {
        return -1;
    } else {
        return res[n][amount];
    }
}
```

## Java
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int n = coins.length;
        int maxVal = Integer.MAX_VALUE;
        int[][] res = new int[n+1][amount+1];
        
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= amount; j++) {  // (i->no of coins,j->amount)
                if (i == 0) { //dont have any coins,to make required amount,hence set to infinity
                    res[i][j] = maxVal;
                }
                if (j == 0) { // means target amount is 0, hence required 0 min no of coins
                    res[i][j] = 0;
                }
                if (i == 1) { // it means, we have only one coin, in array
                    if (j > 0 && coins[0] > 0 && j % coins[0] == 0) { // checking able to make required amount,with that coin
                        res[i][j] = j / coins[0]; // no of coins,required to make that amount
                    } else if (j > 0) {
                        res[i][j] = maxVal; //not able to make that amount, set to infinity
                    }
                }
            }
        }
        
        for (int i = 2; i <= n; i++) {
            for (int j = 1; j <= amount; j++) {
                if (coins[i-1] <= j) { // if curr coin value of array,is less than amount, then it means we can either add that coin(by doing +1 and subtracting that coin value from amount), or can neglect that coin, wil take minimum of this
                    if (res[i][j - coins[i-1]] != maxVal) {
                        res[i][j] = Math.min(1 + res[i][j - coins[i-1]], res[i-1][j]);
                    } else {
                        res[i][j] = res[i-1][j];
                    }
                } else { // coin value is > hence, will wil not add that coin
                    res[i][j] = res[i-1][j];
                }
            }
        }
        
        if (res[n][amount] == maxVal) {
            return -1;
        } else {
            return res[n][amount];
        }
    }
}
```



#### Using 1-D DP

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
