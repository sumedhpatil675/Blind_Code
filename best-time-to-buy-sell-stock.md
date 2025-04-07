# Best Time to Buy and Sell Stock

## Problem Description
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

## Approaches

### 1. Brute Force
* Time complexity: O(n²)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        max_profit = 0
        
        for i in range(len(prices)):
            for j in range(i + 1, len(prices)):
                profit = prices[j] - prices[i]
                max_profit = max(max_profit, profit)
                
        return max_profit
```

#### JavaScript
```javascript
var maxProfit = function(prices) {
    let maxProfit = 0;
    
    for (let i = 0; i < prices.length; i++) {
        for (let j = i + 1; j < prices.length; j++) {
            const profit = prices[j] - prices[i];
            maxProfit = Math.max(maxProfit, profit);
        }
    }
    
    return maxProfit;
};
```

#### Java
```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        
        for (int i = 0; i < prices.length; i++) {
            for (int j = i + 1; j < prices.length; j++) {
                int profit = prices[j] - prices[i];
                maxProfit = Math.max(maxProfit, profit);
            }
        }
        
        return maxProfit;
    }
}
```

### 2. Two Pointers (Sliding Window) ✅
* Time complexity: O(n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        l, r = 0, 1  # left=buy, right=sell
        max_profit = 0
        
        while r < len(prices):
            # Is this a profitable transaction?
            if prices[l] < prices[r]:
                profit = prices[r] - prices[l]
                max_profit = max(max_profit, profit)
            else:
                # If we find a new minimum price, update the buy pointer
                l = r
            r += 1
            
        return max_profit
```

#### JavaScript
```javascript
var maxProfit = function(prices) {
    let left = 0;  // Buy pointer
    let right = 1; // Sell pointer
    let maxProfit = 0;
    
    while (right < prices.length) {
        // Is this a profitable transaction?
        if (prices[left] < prices[right]) {
            const profit = prices[right] - prices[left];
            maxProfit = Math.max(maxProfit, profit);
        } else {
            // If we find a new minimum price, update the buy pointer
            left = right;
        }
        right++;
    }
    
    return maxProfit;
};
```

#### Java
```java
class Solution {
    public int maxProfit(int[] prices) {
        int left = 0;  // Buy pointer
        int right = 1; // Sell pointer
        int maxProfit = 0;
        
        while (right < prices.length) {
            // Is this a profitable transaction?
            if (prices[left] < prices[right]) {
                int profit = prices[right] - prices[left];
                maxProfit = Math.max(maxProfit, profit);
            } else {
                // If we find a new minimum price, update the buy pointer
                left = right;
            }
            right++;
        }
        
        return maxProfit;
    }
}
```

### 3. Dynamic Programming
* Time complexity: O(n)
* Space complexity: O(1)

#### Python
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        max_profit = 0
        min_price = float('inf')
        
        for price in prices:
            # Update the minimum price seen so far
            min_price = min(min_price, price)
            # Calculate potential profit with current price
            profit = price - min_price
            # Update the maximum profit seen so far
            max_profit = max(max_profit, profit)
            
        return max_profit
```

#### JavaScript
```javascript
var maxProfit = function(prices) {
    let maxProfit = 0;
    let minPrice = Infinity;
    
    for (const price of prices) {
        // Update the minimum price seen so far
        minPrice = Math.min(minPrice, price);
        // Calculate potential profit with current price
        const profit = price - minPrice;
        // Update the maximum profit seen so far
        maxProfit = Math.max(maxProfit, profit);
    }
    
    return maxProfit;
};
```

#### Java
```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        int minPrice = Integer.MAX_VALUE;
        
        for (int price : prices) {
            // Update the minimum price seen so far
            minPrice = Math.min(minPrice, price);
            // Calculate potential profit with current price
            int profit = price - minPrice;
            // Update the maximum profit seen so far
            maxProfit = Math.max(maxProfit, profit);
        }
        
        return maxProfit;
    }
}
```
