# [Geek's Training](https://www.geeksforgeeks.org/problems/geeks-training/1)

| Difficulty | Topic               | Companies | Resources   |
| ---------- | ------------------- | --------- | ----------- |
| **Medium** | Dynamic Programming |           | [Video](https://youtu.be/AE39gJYuRog)   |
|            |                     |           | [Article](https://takeuforward.org/data-structure/dynamic-programming-ninjas-training-dp-7/) |

## Description
Geek is going for a training program for n days. He can perform any of these activities: Running, Fighting, and Learning Practice. Each activity has some point on each day. As Geek wants to improve all his skills, he can't do the same activity on two consecutive days. Given a 2D array arr[][] of size n where arr[i][0], arr[i][1], and arr[i][2] represent the merit points for Running, Fighting, and Learning on the i-th day, determine the maximum total merit points Geek can achieve .

**Example 1**
```
Input: arr[]= [[1, 2, 5], [3, 1, 1], [3, 3, 3]]
Output: 11
Explanation: Geek will learn a new move and earn 5 point then on second day he will do running and earn 3 point and on third day he will do fighting and earn 3 points so, maximum merit point will be 11.
```

**Example 2**
```
nput: arr[]= [[1, 1, 1], [2, 2, 2], [3, 3, 3]]
Output: 6
Explanation: Geek can perform any activity each day while adhering to the constraints, in order to maximize his total merit points as 6.
```

**Constraints:**
- `1 <=  n <= 105`   
- `1 <=  arr[i][j] <= 100`


## Using Recursion

### Recursion Tree
arr = {
    {1, 2, 5},
    {3, 1, 1},
    {3, 3, 3}
}

We will follow a top-down approach. We define a function f(day, last) that returns the maximum points that can be obtained by performing the given activity on the given day and the given last activity.

The base case is when it's the first day (day = 0). In this case, we find the maximum points from activities not done last.

The recursive case is when it's not the first day. We try all activities that are not the same as the last activity and take the maximum points. We recursively call the function f(day - 1, activity) for each activity.

For the (n-1)th day, there is no activity done yet, so we use 3 to denote that no activity has been done before.
The recursion tree is as follows:

                                         f(2, 3)
                                    /       |       \
                                f(1, 0)   f(1, 1)   f(1, 2)
                                /   \      /   \      /   \
                           f(0,1) f(0,2) f(0,0) f(0,2) f(0,0) f(0,1)

### Code
```java
class Solution {
    // Helper function to find the maximum points that can be obtained for the given day and last activity
    int helper(int day, int last, int[][] points){
        // Base case. If it's the first day, find the maximum points from activities not done last
        if(day == 0){
            int maxi = 0;
            
            // Iterate over the activities and choose the one which has the maximum points
            // and is not equal to the last activity
            for(int task = 0; task < 3; task ++){
                if(task != last){
                    maxi = Math.max(maxi, points[0][task]);
                }
            }
            
            return maxi;
        }
        
        int maxi = 0;
        
        // Iterate over the activities and choose the one which has the maximum points
        // and is not equal to the last activity
        for(int task = 0; task < 3; task ++){
            if(task != last){
                int point = points[day][task] + helper(day-1, task, points);
                maxi = Math.max(maxi, point);
            }
        }
        return maxi;
    }
    
    // Function to find the maximum points that can be obtained
    public int maximumPoints(int arr[][]) {
        int n = arr.length;
        // call the helper function with the last day and 3 as the last activity
        return helper(n-1, 3, arr);
    }
}
```

### Complexity Analysis

- **Time Complexiy** : `O(3^n)`
    - For each day, you try all 3 possible activities (except the last one).
    - For n days, the number of function calls is O(3^n) in the worst case.
  
- **Space Complexity** : `O(n)`
    - The maximum depth of the recursion stack is O(n) (one call per day).


## Using Dynamic Programming [Memoization]
 
### Code
```java
class Solution {

    // Helper function to determine the maximum points obtainable for the given day and last activity
    int helper(int day, int lastActivity, int[][] points, int[][] dp) {
        // Base case: If it's the first day, find the maximum points from activities not done last
        if (day == 0) {
            int maxPoints = 0;
            for (int task = 0; task < 3; task++) {
                if (task != lastActivity) {
                    maxPoints = Math.max(maxPoints, points[0][task]);
                }
            }
            return maxPoints;
        }

        // Check if the result is already computed
        if (dp[day][lastActivity] != -1) {
            return dp[day][lastActivity];
        }

        int maxPoints = 0;

        // Iterate through all possible tasks
        for (int task = 0; task < 3; task++) {
            // Ensure the current task is not the same as the last activity
            if (task != lastActivity) {
                // Calculate the points for the current task and recursively find points for the previous day
                int currentPoints = points[day][task] + helper(day - 1, task, points, dp);
                maxPoints = Math.max(maxPoints, currentPoints);
            }
        }

        // Memoize the computed result
        return dp[day][lastActivity] = maxPoints;
    }

    // Main function to calculate the maximum points
    public int maximumPoints(int[][] arr) {
        int n = arr.length;
        int[][] dp = new int[n][4]; // Memoization table

        // Initialize dp array with -1 indicating uncomputed states
        for (int i = 0; i < n; i++) {
            Arrays.fill(dp[i], -1);
        }

        // Call the helper function starting from the last day with no last activity constraint
        return helper(n - 1, 3, arr, dp);
    }
}
```

### Complexity Analysis

- **Time Complexiy** : ` O(n * 4 * 3)` → `O(n)` (since 4 and 3 are constants)
  - There are n days and 4 possible values for lastActivity (0, 1, 2, 3).
  - Each state (day, lastActivity) is computed only once.
  - For each state, you try up to 3 activities.
  
- **Space Complexity** : `O(n)`
  - The DP table is of size n x 4.
  - The recursion stack can go up to O(n) deep.