# [Aggressive Cows](https://www.geeksforgeeks.org/problems/aggressive-cows/0)

| Difficulty | Topic         | Companies | Resources   |
| ---------- | ------------- | --------- | ----------- |
| **Medium** | Binary Search |           | [Video](https://youtu.be/R_Mfw4ew-Vo)   |
|            |               |           | [Article](https://www.geeksforgeeks.org/assign-stalls-to-k-cows-to-maximize-the-minimum-distance-between-them/) |

## Description

You are given an array `stalls[]` representing the positions of stalls, where each element is unique. You are also given an integer `k` representing the number of aggressive cows. Your task is to assign stalls to `k` cows such that the minimum distance between any two cows is maximized.

**Examples**

```
Example 1:
Input: stalls[] = [1, 2, 4, 8, 9], k = 3
Output: 3
Explanation: The first cow can be placed at stalls[0], 
the second cow can be placed at stalls[2] and 
the third cow can be placed at stalls[3]. 
The minimum distance between cows, in this case, is 3, which also is the largest among all possible ways.
```

```
Example 2:
Input: stalls[] = [10, 1, 2, 7, 5], k = 3
Output: 4
Explanation: The first cow can be placed at stalls[0],
the second cow can be placed at stalls[1] and
the third cow can be placed at stalls[4].
The minimum distance between cows, in this case, is 4, which also is the largest among all possible ways.
```

```
Example 3:
Input: stalls[] = [2, 12, 11, 3, 26, 7], k = 5
Output: 1
Explanation: Each cow can be placed in any of the stalls, as the no. of stalls are exactly equal to the number of cows.
The minimum distance between cows, in this case, is 1, which also is the largest among all possible ways.
```

**Constraints:**
- `2 <= stalls.size() <= 10^6`
- `0 <= stalls[i] <= 10^8`
- `2 <= k <= stalls.size()`

## [Naive Approach] By iterating over all possible distances
The idea is to iterate over all possible distances starting from 1 up to the difference between the farthest stalls. First, we sort the array to ensure the stalls are in the correct sequence. Then, for each distance, we try to place the cows greedily – placing the first cow in the first stall and the next ones only if the gap from the last placed cow is at least the current distance. If all cows can be placed for a certain distance, we update our result. The process continues until all distances are checked.

### Code
```java
class Solution {
    private static int isPossible(int[] arr, int dis){
        // cow1 assigned
        int cntCows = 1; 

        // cow1 would be at first place always
        int lastAssigned = arr[0]; 

        for(int i = 1; i < arr.length; i ++){
            if(arr[i] - lastAssigned >= dis){
                cntCows ++; // another cow assigned
                lastAssigned = arr[i]; // last assigned cow
            }
        }
        
        return cntCows;
    }
    
    public static int aggressiveCows(int[] stalls, int k) {
        int n = stalls.length;
        
        // sort stalls
        Arrays.sort(stalls); 
        int maxiDis = stalls[n-1] - stalls[0]; // maxi - mini, maximum distance
        int ans = 1; // result 
        
        for(int i = 1; i <= maxiDis; i ++){
            int cntCows = isPossible(stalls, i);
            
            if(cntCows >= k){
                // if possible then store & go for next distance to calculate maximum 
                ans = i;   
            }
        }
        
        return ans;
    }
}
```

### Complexity Analysis
- Time Complexity : `O(n log n) + O(maxi - mini) * O(n)` ~ `O(n ^ 2)`
- Space Complexity : `O(1)`


## [Optimal Approach] Using Binary Search

### Code
```java
class Solution {
    // check if we can assign `k` cows with minimum distance of `dis`
    private static int isPossible(int[] arr, int dis){
        // cow1 assigned
        int cntCows = 1; 
        
        // cow1 would be at first place always
        int lastAssigned = arr[0]; 

        for(int i = 1; i < arr.length; i ++){
            if(arr[i] - lastAssigned >= dis){
                cntCows ++; // another cow assigned
                lastAssigned = arr[i]; // last assigned cow
            }
        }
        
        return cntCows;
    }
    
    public static int aggressiveCows(int[] stalls, int k) {
        int n = stalls.length;
        
        // sort stalls
        Arrays.sort(stalls); 
        int low = 1, high = stalls[n-1] - stalls[0]; // maxi - mini, maximum distance
        int ans = 1; // result 
        
        // Binary Search
        while(low <= high){
            int mid = (low + high) >> 1; // calculate mid
            int cntCows = isPossible(stalls, mid);
            
            if(cntCows >= k){ // want maximum distance
                ans = mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        
        return ans;
    }
}
```

### Complexity Analysis

- Time Complexity : `O(n log n) + O(d log d) * O(n)`, where `d` is `high = stalls[n-1] - stalls[0]`
- Space Complexity : `O(1)`
