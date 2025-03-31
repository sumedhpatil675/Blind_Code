# Cheapest Flights Within K Stops

There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei] indicates that there is a flight from city fromi to city toi with cost pricei.

You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

## Approaches

### 1. Breadth-First Search (BFS)
- **Time complexity**: O(V + E * K)
- **Space complexity**: O(V)

Where V is the number of vertices (cities) and E is the number of edges (flights).

#### Python
```python
from collections import defaultdict, deque

def findCheapestPrice(n, flights, src, dst, k):
    # Build the adjacency list
    graph = defaultdict(list)
    for u, v, w in flights:
        graph[u].append((v, w))
    
    # BFS queue: (node, cost, stops)
    queue = deque([(src, 0, 0)])
    
    # Keep track of minimum cost to reach each node
    min_cost = [float('inf')] * n
    min_cost[src] = 0
    
    while queue:
        node, cost, stops = queue.popleft()
        
        # If we've reached the destination, return the cost
        if node == dst:
            return cost
        
        # If we've used up all stops, don't explore further
        if stops > k:
            continue
        
        # Explore neighbors
        for neighbor, price in graph[node]:
            new_cost = cost + price
            
            # Only consider this path if it's better than what we've seen
            # or we have fewer stops
            if new_cost < min_cost[neighbor] or stops < k:
                min_cost[neighbor] = min(min_cost[neighbor], new_cost)
                queue.append((neighbor, new_cost, stops + 1))
    
    # If we can't reach the destination within k stops
    return -1 if min_cost[dst] == float('inf') else min_cost[dst]
```

#### JavaScript
```javascript
function findCheapestPrice(n, flights, src, dst, k) {
    // Build the adjacency list
    const graph = Array(n).fill().map(() => []);
    for (const [from, to, price] of flights) {
        graph[from].push([to, price]);
    }
    
    // BFS queue: [node, cost, stops]
    const queue = [[src, 0, 0]];
    
    // Keep track of minimum cost to reach each node
    const minCost = Array(n).fill(Infinity);
    minCost[src] = 0;
    
    while (queue.length > 0) {
        const [node, cost, stops] = queue.shift();
        
        // If we've reached the destination, return the cost
        if (node === dst) {
            return cost;
        }
        
        // If we've used up all stops, don't explore further
        if (stops > k) {
            continue;
        }
        
        // Explore neighbors
        for (const [neighbor, price] of graph[node]) {
            const newCost = cost + price;
            
            // Only consider this path if it's better than what we've seen
            // or we have fewer stops
            if (newCost < minCost[neighbor] || stops < k) {
                minCost[neighbor] = Math.min(minCost[neighbor], newCost);
                queue.push([neighbor, newCost, stops + 1]);
            }
        }
    }
    
    // If we can't reach the destination within k stops
    return minCost[dst] === Infinity ? -1 : minCost[dst];
}
```

#### Java
```java
public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
    // Build the adjacency list
    Map<Integer, List<int[]>> graph = new HashMap<>();
    for (int i = 0; i < n; i++) {
        graph.put(i, new ArrayList<>());
    }
    
    for (int[] flight : flights) {
        int from = flight[0];
        int to = flight[1];
        int price = flight[2];
        graph.get(from).add(new int[]{to, price});
    }
    
    // BFS queue: [node, cost, stops]
    Queue<int[]> queue = new LinkedList<>();
    queue.offer(new int[]{src, 0, 0});
    
    // Keep track of minimum cost to reach each node
    int[] minCost = new int[n];
    Arrays.fill(minCost, Integer.MAX_VALUE);
    minCost[src] = 0;
    
    while (!queue.isEmpty()) {
        int[] curr = queue.poll();
        int node = curr[0];
        int cost = curr[1];
        int stops = curr[2];
        
        // If we've reached the destination, return the cost
        if (node == dst) {
            return cost;
        }
        
        // If we've used up all stops, don't explore further
        if (stops > k) {
            continue;
        }
        
        // Explore neighbors
        for (int[] neighbor : graph.get(node)) {
            int nextNode = neighbor[0];
            int price = neighbor[1];
            int newCost = cost + price;
            
            // Only consider this path if it's better than what we've seen
            // or we have fewer stops
            if (newCost < minCost[nextNode] || stops < k) {
                minCost[nextNode] = Math.min(minCost[nextNode], newCost);
                queue.offer(new int[]{nextNode, newCost, stops + 1});
            }
        }
    }
    
    // If we can't reach the destination within k stops
    return minCost[dst] == Integer.MAX_VALUE ? -1 : minCost[dst];
}
```

### 2. Dijkstra's Algorithm
- **Time complexity**: O(E * log(V * K))
- **Space complexity**: O(V * K)

#### Python
```python
import heapq
from collections import defaultdict

def findCheapestPrice(n, flights, src, dst, k):
    # Build the adjacency list
    graph = defaultdict(list)
    for u, v, w in flights:
        graph[u].append((v, w))
    
    # Priority queue: (cost, node, stops)
    pq = [(0, src, 0)]  # (cost, node, stops)
    
    # Dictionary to store the minimum cost to reach a node with a certain number of stops
    min_cost = {}  # (node, stops): cost
    
    while pq:
        cost, node, stops = heapq.heappop(pq)
        
        # If we've reached the destination, return the cost
        if node == dst:
            return cost
        
        # If we've used up all stops, don't explore further
        if stops > k:
            continue
        
        # If we've already found a better path to this node with the same or fewer stops, skip
        if (node, stops) in min_cost and cost >= min_cost[(node, stops)]:
            continue
        
        min_cost[(node, stops)] = cost
        
        # Explore neighbors
        for neighbor, price in graph[node]:
            heapq.heappush(pq, (cost + price, neighbor, stops + 1))
    
    # If we can't reach the destination within k stops
    return -1
```

#### JavaScript
```javascript
function findCheapestPrice(n, flights, src, dst, k) {
    // Build the adjacency list
    const graph = Array(n).fill().map(() => []);
    for (const [from, to, price] of flights) {
        graph[from].push([to, price]);
    }
    
    // Priority queue: [cost, node, stops]
    const pq = [[0, src, 0]];  // Using array as a simple priority queue
    
    // Map to store the minimum cost to reach a node with a certain number of stops
    const minCost = new Map();  // key: 'node-stops', value: cost
    
    while (pq.length > 0) {
        // Extract minimum element (not efficient, but simplified)
        pq.sort((a, b) => a[0] - b[0]);
        const [cost, node, stops] = pq.shift();
        
        // If we've reached the destination, return the cost
        if (node === dst) {
            return cost;
        }
        
        // If we've used up all stops, don't explore further
        if (stops > k) {
            continue;
        }
        
        // Generate a key for the current state
        const key = `${node}-${stops}`;
        
        // If we've already found a better path to this node with the same or fewer stops, skip
        if (minCost.has(key) && cost >= minCost.get(key)) {
            continue;
        }
        
        minCost.set(key, cost);
        
        // Explore neighbors
        for (const [neighbor, price] of graph[node]) {
            pq.push([cost + price, neighbor, stops + 1]);
        }
    }
    
    // If we can't reach the destination within k stops
    return -1;
}
```

#### Java
```java
public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
    // Build the adjacency list
    Map<Integer, List<int[]>> graph = new HashMap<>();
    for (int i = 0; i < n; i++) {
        graph.put(i, new ArrayList<>());
    }
    
    for (int[] flight : flights) {
        int from = flight[0];
        int to = flight[1];
        int price = flight[2];
        graph.get(from).add(new int[]{to, price});
    }
    
    // Priority queue: [cost, node, stops]
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> Integer.compare(a[0], b[0]));
    pq.offer(new int[]{0, src, 0});
    
    // Map to store the minimum cost to reach a node with a certain number of stops
    Map<String, Integer> minCost = new HashMap<>();
    
    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int cost = curr[0];
        int node = curr[1];
        int stops = curr[2];
        
        // If we've reached the destination, return the cost
        if (node == dst) {
            return cost;
        }
        
        // If we've used up all stops, don't explore further
        if (stops > k) {
            continue;
        }
        
        // Generate a key for the current state
        String key = node + "-" + stops;
        
        // If we've already found a better path to this node with the same or fewer stops, skip
        if (minCost.containsKey(key) && cost >= minCost.get(key)) {
            continue;
        }
        
        minCost.put(key, cost);
        
        // Explore neighbors
        for (int[] neighbor : graph.get(node)) {
            int nextNode = neighbor[0];
            int price = neighbor[1];
            pq.offer(new int[]{cost + price, nextNode, stops + 1});
        }
    }
    
    // If we can't reach the destination within k stops
    return -1;
}
```

### 3. Bellman-Ford Algorithm
- **Time complexity**: O(K * E)
- **Space complexity**: O(V)

#### Python
```python
def findCheapestPrice(n, flights, src, dst, k):
    # Initialize distances with infinity
    prices = [float('inf')] * n
    prices[src] = 0
    
    # Relax edges k+1 times
    for i in range(k + 1):
        # Create a copy of prices to avoid updating based on prices updated in this iteration
        temp_prices = prices.copy()
        
        for u, v, w in flights:
            if prices[u] != float('inf') and prices[u] + w < temp_prices[v]:
                temp_prices[v] = prices[u] + w
        
        prices = temp_prices
    
    # Return the result
    return -1 if prices[dst] == float('inf') else prices[dst]
```

#### JavaScript
```javascript
function findCheapestPrice(n, flights, src, dst, k) {
    // Initialize distances with infinity
    const prices = Array(n).fill(Infinity);
    prices[src] = 0;
    
    // Relax edges k+1 times
    for (let i = 0; i <= k; i++) {
        // Create a copy of prices to avoid updating based on prices updated in this iteration
        const tempPrices = [...prices];
        
        for (const [from, to, price] of flights) {
            if (prices[from] !== Infinity && prices[from] + price < tempPrices[to]) {
                tempPrices[to] = prices[from] + price;
            }
        }
        
        // Update prices for the next iteration
        prices.splice(0, n, ...tempPrices);
    }
    
    // Return the result
    return prices[dst] === Infinity ? -1 : prices[dst];
}
```

#### Java
```java
public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
    // Initialize distances with infinity
    int[] prices = new int[n];
    Arrays.fill(prices, Integer.MAX_VALUE);
    prices[src] = 0;
    
    // Relax edges k+1 times
    for (int i = 0; i <= k; i++) {
        // Create a copy of prices to avoid updating based on prices updated in this iteration
        int[] tempPrices = Arrays.copyOf(prices, n);
        
        for (int[] flight : flights) {
            int from = flight[0];
            int to = flight[1];
            int price = flight[2];
            
            if (prices[from] != Integer.MAX_VALUE && prices[from] + price < tempPrices[to]) {
                tempPrices[to] = prices[from] + price;
            }
        }
        
        // Update prices for the next iteration
        prices = tempPrices;
    }
    
    // Return the result
    return prices[dst] == Integer.MAX_VALUE ? -1 : prices[dst];
}
```