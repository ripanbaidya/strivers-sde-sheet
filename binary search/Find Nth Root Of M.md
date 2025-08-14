# [Find Nth Root Of M](https://www.naukri.com/code360/problems/1062679?topList=striver-sde-sheet-problems&utm_source=striver&utm_medium=website&leftPanelTabValue=PROBLEM)

| Difficulty | Topic         | Companies | Resources   |
| ---------- | ------------- | --------- | ----------- |
| **Medium** | Binary Search |           | [Video](https://youtu.be/WjpswYrS2nY) |
|            |               |           | [Article](https://www.geeksforgeeks.org/n-th-root-number/) |

## Description
Given two numbers n and m, find the n-th root of m. In mathematics, the n-th root of a number m is a real number that, when raised to the power of n, gives m. If no such real number exists, return -1.

**Examples:** 

```
Example 1:
Input: n = 2, m = 9
Output: 3
Explanation: 32 = 9

Example 2:
Input: n = 3, m = 9
Output: -1
Explanation: 3rd root of 9 is not integer.
```

**Constraints:**
- 1 <= n <= 30
- 1 <= m <= 10^9

## [Optimal Approach] Using Binary Search

### Intution
We need to find the Nth root of M (i.e., x such that x^N = M).
A brute-force approach would iterate from 1 to M and check if x^N == M, but this is inefficient. 
Instead, we can optimize it using binary search.

### Approach
We know the Nth root of M lies between 1 and M.

* Initialize low = 1 and high = M.
* Perform binary search while low <= high:
* Compute mid = (low + high) / 2.
* Calculate num = pow(mid, n).
    * If num == M, return mid.
    * If num > M, set high = mid - 1.
    * If num < M, set low = mid + 1.

* If no valid mid is found, return -1.

### Code
```java
public class Solution {
    /**
     * Finds the nth root of a given number m.
     */
    public static int nthRoot(int n, int m) {
        int lowerBound = 1;
        int upperBound = m / 2;

        // Binary search for the nth root
        while (lowerBound <= upperBound) {
            int mid = (lowerBound + upperBound) / 2;

            // Calculate the nth power of mid
            long midPowerN = (long) Math.pow(mid, n);

            // If midPowerN equals m, we have found the root
            if (midPowerN == m) {
                return mid;
            }

            // If midPowerN is greater than m, reduce the upper bound
            if (midPowerN > m) {
                upperBound = mid - 1;
            }

            // If midPowerN is less than m, increase the lower bound
            else {
                lowerBound = mid + 1;
            }
        }

        // If no valid root is found, return -1
        return -1;
    }
}
```

### Complexity Analysis

- **Time Complexity** : `O(log m) * O(log n)`, O(log m) for binary search and O(log n) is for Math.pow(x, x)
- **Space Complexity** : `O(1)`