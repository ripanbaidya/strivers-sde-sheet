# [Frog Jump](https://www.geeksforgeeks.org/problems/geek-jump/1)

| Difficulty | Topic               | Companies | Resources   |
| ---------- | ------------------- | --------- | ----------- |
| **Medium** | Dynamic Programming |           | [Video]()   |
|            |                     |           | [Article]() |

## Description
Given an integer array height[] where height[i] represents the height of the i-th stair, a frog starts from the first stair and wants to reach the top. From any stair i, the frog has two options: it can either jump to the (i+1)th stair or the (i+2)th stair. The cost of a jump is the absolute difference in height between the two stairs. Determine the minimum total cost required for the frog to reach the top.

**Examples**

```
Example 1:
Input: heights[] = [20, 30, 40, 20] 
Output: 20
Explanation:  Minimum cost is incurred when the frog jumps from stair 0 to 1 then 1 to 3:
jump from stair 0 to 1: cost = |30 - 20| = 10
jump from stair 1 to 3: cost = |20-30|  = 10
Total Cost = 10 + 10 = 20
```

```
Example 2:
Input: heights[] = [30, 20, 50, 10, 40]
Output: 30
Explanation: Minimum cost will be incurred when frog jumps from stair 0 to 2 then 2 to 4:
jump from stair 0 to 2: cost = |50 - 30| = 20
jump from stair 2 to 4: cost = |40-50|  = 10
Total Cost = 20 + 10 = 30
```

**Constraints:**
- 1 <= height.size() <= 105
- 0 <= height[i]<=104


## Using Dynamic Programming - Memoization

### Code
```java
class Solution {
    // Helper function to calculate the minimum cost to reach the top stair
    private int helper(int index, int[] dp, int[] height) {
        // Base case: when we reach the first stair, cost is zero
        if (index == 0) return 0;
        
        // Return already computed value for dp[index] if available
        if (dp[index] != 0) return dp[index];
        
        // Calculate cost of jumping from the previous stair
        int firstJump = helper(index - 1, dp, height) + Math.abs(height[index] - height[index - 1]);
        
        // Initialize secondJump to max value; calculate only if index > 1
        int secondJump = Integer.MAX_VALUE;
        if (index > 1) {
            secondJump = helper(index - 2, dp, height) + Math.abs(height[index] - height[index - 2]);
        }
        
        // Store the minimum cost in dp[index]
        return dp[index] = Math.min(firstJump, secondJump);
    }

    // Function to calculate the minimum cost to reach the top stair
    public int minCost(int[] height) {
        int n = height.length;
        int[] dp = new int[n]; // DP array for memoization
        return helper(n - 1, dp, height); // Start from the last stair
    }
}
```

### Complexity Analysis

- **Time Complexiy** : `O(n)`
- **Space Complexity** : `O(n) + O(n)`, stack space and for dp array