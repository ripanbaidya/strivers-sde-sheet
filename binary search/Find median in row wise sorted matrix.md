# [Find median in row wise sorted matrix](https://www.geeksforgeeks.org/find-median-row-wise-sorted-matrix/)

| Difficulty | Topic         | Companies | Resources   |
| ---------- | ------------- | --------- | ----------- |
| **Medium** | Binary Search |           | [Video](https://youtu.be/Q9wXgdxJq48?si=KZz_SyIRgM1aD8Zh)   |
|            |               |           | [Article](https://www.geeksforgeeks.org/find-median-row-wise-sorted-matrix/) |

## Description
Given a row-wise sorted matrix where the number of rows and columns is always odd, find the median of the matrix.

**Examples:**
```
Input: mat = [[1, 3, 5], [2, 6, 9], [3, 6, 9]]
Output: 5
Explanation: Sorting matrix elements gives us {1,2,3,3,5,6,6,9,9}. Hence, 5 is median. 

Input: mat = [[1], [2], [3]]
Output: 2
Explanation: Sorting matrix elements gives us {1,2,3}. Hence, 2 is median

Input: mat = [[3], [5], [8]]
Output: 5
Explanation: Sorting matrix elements gives us {3,5,8}. Hence, 5 is median.
```

**Constraints:**

- 1 <= mat.size(), mat[0].size() <= 400
- 1 <= mat[i][j] <= 2000


## [Naive Approach] Using Sorting – O(n * m * log(n * m)) Time and O(n * m) Space

The idea is to store all the elements of the given matrix in a new array of size r x c. Then sort the array and find the median element.

### Code
```java
class Solution{
    int median(int mat[][]) {
      
        // Flatten the matrix into a 1D array
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < mat.length; i++) {
            for (int j = 0; j < mat[0].length; j++) {
                list.add(mat[i][j]);
            }
        }

        // Sort the list
        Collections.sort(list);

        // Find and return the median element
        int mid = list.size() / 2;
        return list.get(mid);
    }
}
```

### Complexity Analysis

- **Time Complexity** : `O(n * m * log(n * m))`
- **Space Complexity**  : `O(n * m)`


## [Expected Approach] – Using Binary Search – O(n * log(m) * log(maxVal – minVal)) Time and O(1) Space

An efficient approach for this problem is to use binary search algorithm. The idea is that for a number x to be median there should be exactly r * c / 2 numbers that are less than this number. So, we apply binary search on search space [minimum Element to maximum Element] and try to find the count of numbers less than mid. If this count is less than r * c / 2 then search in upper half to increase the count, otherwise search in lower half to decrease the count.

### Code
```java
class Solution{
    int median(int mat[][]) {
        int n = mat.length;
        int m = mat[0].length;

        // Initializing the minimum and maximum values
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;

        // Iterating through each row of the matrix
        for (int i = 0; i < n; i++) {
          
            // Updating the minimum value if current element is smaller
            if (mat[i][0] < min) 
              	min = mat[i][0];

            // Updating the maximum value if current element is larger
            if (mat[i][m - 1] > max) 
              	max = mat[i][m - 1];
        }

        // Calculating the desired position of the median
        int desired = (n * m + 1) / 2;

        // Using binary search to find the median value
        while (min < max) {
          
            // Calculating the middle value
            int mid = min + (max - min) / 2;

            // Counting the number of elements less than or equal to mid
            int place = 0;
            for (int i = 0; i < n; i++) {
                place += upperBound(mat[i], mid);
            }

            // Updating the search range based on the count
            if (place < desired) {
                min = mid + 1;
            } else {
                max = mid;
            }
        }

        // Returning the median value
        return min;
    }

    // Helper function to find the upper bound of a number in a row
    int upperBound(int[] row, int num) {
        int low = 0, high = row.length;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (row[mid] <= num) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}
```

### Complexity Analysis

- **Time Complexity** : `O(n * log(m) * log(maxVal – minVal))`, the upper bound function will take log(m) time and is performed for each row. And binary search is performed from minVal to maxVal. 
- **Space Complexity**  : `O(1)`
