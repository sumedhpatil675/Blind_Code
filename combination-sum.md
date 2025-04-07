# Combination Sum

Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

## Approaches

### 1. Backtracking ✅

**Time complexity:** O(2^(t/m))  
**Space complexity:** O(t/m)  
where t is the given target and m is the minimum value in the array.

#### Python
```python
class Solution:
    def combinationSum(self, nums: List[int], target: int) -> List[List[int]]:
        res = []
        
        def dfs(i, cur, total):
            if total == target:
                res.append(cur.copy())
                return
                
            if i >= len(nums) or total > target:
                return
                
            cur.append(nums[i])
            dfs(i, cur, total + nums[i])
            cur.pop()
            dfs(i + 1, cur, total)
            
        dfs(0, [], 0)
        return res
```

#### JavaScript
```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum = function(candidates, target) {
    const result = [];
    
    function dfs(i, current, total) {
        if (total === target) {
            result.push([...current]);
            return;
        }
        
        if (i >= candidates.length || total > target) {
            return;
        }
        
        current.push(candidates[i]);
        dfs(i, current, total + candidates[i]);
        current.pop();
        dfs(i + 1, current, total);
    }
    
    dfs(0, [], 0);
    return result;
};
```

#### Java
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(candidates, 0, target, new ArrayList<>(), result);
        return result;
    }
    
    private void backtrack(int[] candidates, int start, int target, List<Integer> current, List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        if (target < 0) {
            return;
        }
        
        for (int i = start; i < candidates.length; i++) {
            current.add(candidates[i]);
            backtrack(candidates, i, target - candidates[i], current, result);
            current.remove(current.size() - 1);
        }
    }
}
```

### 2. Backtracking (Optimal)

**Time complexity:** O(2^(t/m))  
**Space complexity:** O(t/m)  
where t is the given target and m is the minimum value in the array.

#### Python
```python
class Solution:
    def combinationSum(self, nums: List[int], target: int) -> List[List[int]]:
        res = []
        nums.sort()
        
        def dfs(i, cur, total):
            if total == target:
                res.append(cur.copy())
                return
                
            for j in range(i, len(nums)):
                if total + nums[j] > target:
                    return
                    
                cur.append(nums[j])
                dfs(j, cur, total + nums[j])
                cur.pop()
                
        dfs(0, [], 0)
        return res
```

#### JavaScript
```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum = function(candidates, target) {
    const result = [];
    candidates.sort((a, b) => a - b);
    
    function dfs(index, current, total) {
        if (total === target) {
            result.push([...current]);
            return;
        }
        
        for (let i = index; i < candidates.length; i++) {
            if (total + candidates[i] > target) {
                break;
            }
            
            current.push(candidates[i]);
            dfs(i, current, total + candidates[i]);
            current.pop();
        }
    }
    
    dfs(0, [], 0);
    return result;
};
```

#### Java
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(candidates);
        backtrack(candidates, 0, target, new ArrayList<>(), result);
        return result;
    }
    
    private void backtrack(int[] candidates, int start, int target, List<Integer> current, List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        for (int i = start; i < candidates.length; i++) {
            if (target < candidates[i]) {
                break;
            }
            
            current.add(candidates[i]);
            backtrack(candidates, i, target - candidates[i], current, result);
            current.remove(current.size() - 1);
        }
    }
}
```
