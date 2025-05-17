# [Rat in a Maze Problem - I](https://www.geeksforgeeks.org/problems/rat-in-a-maze-problem/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Backtracking | Google, Amazon      | [Video](https://youtu.be/bLGZhJlt4y0?si=uQp2GpN65W8oDnu6)   |
|            | Arrays       | Microsoft, Facebook | [Article](https://www.geeksforgeeks.org/rat-in-a-maze/) |

## Description
Consider a rat placed at position (0, 0) in an n x n square matrix mat[][]. The rat's goal is to reach the destination at position (n-1, n-1). The rat can move in four possible directions: 'U'(up), 'D'(down), 'L' (left), 'R' (right).

The matrix contains only two possible values:

- 0: A blocked cell through which the rat cannot travel.
- 1: A free cell that the rat can pass through.
Your task is to find all possible paths the rat can take to reach the destination, starting from (0, 0) and ending at (n-1, n-1), under the condition that the rat cannot revisit any cell along the same path. Furthermore, the rat can only move to adjacent cells that are within the bounds of the matrix and not blocked.
If no path exists, return an empty list.

**Note:** Return the final result vector in lexicographically smallest order.


![Input](https://www.geeksforgeeks.org/wp-content/uploads/ratinmaze_filled11.png) ==> 
![Output](https://www.geeksforgeeks.org/wp-content/uploads/ratinmaze_filled_path1.png)

**Example:**
```
Example 1:
Input: mat[][] = [[1, 0, 0, 0], [1, 1, 0, 1], [1, 1, 0, 0], [0, 1, 1, 1]]
Output: ["DDRDRR", "DRDDRR"]
Explanation: The rat can reach the destination at (3, 3) from (0, 0) by two paths - DRDDRR and DDRDRR, when printed in sorted order we get DDRDRR DRDDRR.

Example 2:
Input: mat[][] = [[1, 0], [1, 0]]
Output: []
Explanation: No path exists as the destination cell is blocked.

Example 3:
Input: mat = [[1, 1, 1], [1, 0, 1], [1, 1, 1]]
Output: ["DDRR", "RRDD"]
Explanation: The rat has two possible paths to reach the destination: 1. "DDRR" 2. "RRDD", These are returned in lexicographically sorted order.
```


## [Optimal Approach]

### Code
```java
class Solution {
    private void helper(int r, int c, ArrayList<ArrayList<Integer>> mat, int n,
                        ArrayList<String> ans, String path, int[][] vis, int[] di, int[] dj) {
        
        // If the bottom-right corner is reached, store the path and return
        if (r == n - 1 && c == n - 1) {
            ans.add(path);
            return;
        }

        // Define movement directions in lexicographical order
        String move = "DLRU"; // Down, Left, Right, Up
        for (int i = 0; i < 4; i++) {
            int nexti = r + di[i];
            int nextj = c + dj[i];

            // Check if the next move is within bounds and is a valid path
            if (nexti >= 0 && nextj >= 0 && nexti < n && nextj < n &&
                vis[nexti][nextj] == 0 && mat.get(nexti).get(nextj) == 1) {

                // Mark cell as visited
                vis[r][c] = 1;
                
                // Recur for the next move
                helper(nexti, nextj, mat, n, ans, path + move.charAt(i), vis, di, dj);
                
                // Backtrack - Unmark cell as visited
                vis[r][c] = 0;
            }
        }
    }

    public ArrayList<String> findPath(ArrayList<ArrayList<Integer>> mat) {
        int n = mat.size();
        ArrayList<String> paths = new ArrayList<>();

        // If the start or destination is blocked, return empty paths
        if (n == 0 || mat.get(0).get(0) == 0 || mat.get(n - 1).get(n - 1) == 0) 
            return paths;

        int[][] vis = new int[n][n]; // Visited array
        String path = "";

        // Movement directions: Down, Left, Right, Up
        int[] di = {1, 0, 0, -1};
        int[] dj = {0, -1, 1, 0};

        // Start DFS from (0,0)
        helper(0, 0, mat, n, paths, path, vis, di, dj);

        return paths;
    }
}
```

### Complexity Analysis

- **Time Complexity**: `O(4^(n²))` 
```
In the worst case, each cell can branch in 4 directions  
Up to `n²` cells explored in the recursion tree  
```

- **Space Complexity**: `O(n²)`
```
`O(n²)` for the visited matrix  
`O(n)` recursion stack depth (max path length in `n × n` grid)  
`O(k × n²)` if storing all paths (`k` = number of valid paths, each up to `n²` length)
```

