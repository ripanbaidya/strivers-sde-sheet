# [Count Inversions in an Array](https://www.geeksforgeeks.org/problems/inversion-of-array-1587115620/1)

| Difficulty    | Topics    | Companies | Video                                                 |
| --------------|-----------|-----------|------------------------------------------------------ |
| **Hard**      | Array     |           | [Video](https://youtu.be/AseUmwVNaoY?si=XMKUFJctNCdxFVFW)|
|               |           |           | [Article](https://www.geeksforgeeks.org/inversion-count-in-array-using-merge-sort/) |

`Follow Up` : [3193. Count the Number of Inversions](https://leetcode.com/problems/count-the-number-of-inversions/description/)


## Description

Given an integer array arr[] of size n, find the inversion count in the array. Two array elements arr[i] and arr[j] form an inversion if arr[i] > arr[j] and i < j.

Note: Inversion Count for an array indicates that how far (or close) the array is from being sorted. If the array is already sorted, then the inversion count is 0, but if the array is sorted in reverse order, the inversion count is maximum. 

```
Input: arr[] = {4, 3, 2, 1}
Output: 6
Explanation: 
```

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20241025195915009148/inversion-count.webp" height=220 width=320>

```
Input: arr[] = {1, 2, 3, 4, 5}
Output: 0
Explanation: There is no pair of indexes (i, j) exists in the given array such that arr[i] > arr[j] and i < j


Input: arr[] = {10, 10, 10}
Output: 0
```

**Constraints:**

- `1 <= N <= 10^5`
- `1 <= ARR[i] <= 10^9`

## [Naive Approach] Using Two Nested Loops – O(n^2) Time and O(1) Space

### Intuition
The brute force approach involves checking every possible pair `(i, j)` in the array and counting the number of pairs where `ARR[i] > ARR[j]` and `i < j`.

### Approach
1. Use two nested loops:
   - The outer loop iterates from `i = 0` to `i = n-1`.
   - The inner loop iterates from `j = i+1` to `j = n-1`.
2. For each pair `(i, j)`, check if `ARR[i] > ARR[j]`.
3. If the condition is satisfied, increment the inversion count.

### Code
```java
public class Solution {
    public static long getInversions(long arr[], int n) {
        long cnt = 0;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (arr[i] > arr[j]) {
                    cnt++; // Pair found
                }
            }
        }
        return cnt; // Total no of Pairs
    }
}
```
### Complexity Analysis
- **Time Complexity**: `O(n^2)`  
- **Space Complexity**: `O(1)`  

## [Expected Approach] Using Merge Sort – O(n*log n) Time and O(n) Space

### Intuition
The optimal solution uses a **modified merge sort** to count inversions efficiently. The idea is to divide the array into two halves, recursively count inversions in each half, and then count the inversions that occur when merging the two halves.

### Approach
1. **Divide**: Split the array into two halves.
2. **Conquer**: Recursively count inversions in the left and right halves.
3. **Combine**: While merging the two halves, count the inversions where an element in the left half is greater than an element in the right half.
4. Use a temporary array to store the merged result.

### Code
```java
public class Solution {
    /**
     * Do not use Global Variable in Order to Return answer.
     * Global Variable is highly Discouraged.  
     */
    public static int merge(long[] arr, int low, int mid, int high) {
        // this declaration would result O(N^2) due to the inefficient temp array
        // long[] temp = new long[arr.length];

        long[] temp = new long[high-low+1]; // Allocate only necessary space 

        int k = 0;  // track temp
        int left = low, right = mid + 1;

        int count = 0; // Count total number of inversions
        while (left <= mid && right <= high) {
            if (arr[left] <= arr[right]) {
                temp[k++] = arr[left++];
            } else {
                temp[k++] = arr[right++];
                count += (mid - left + 1); // Count inversions
            }
        }

        // Copy remaining elements from the left half
        while (left <= mid) {
            temp[k++] = arr[left++];
        }

        // Copy remaining elements from the right half
        while (right <= high) {
            temp[k++] = arr[right++];
        }

        // Copy from temp to the original array
        for (int i = low; i <= high; i++) {
            arr[i] = temp[i - low];
        }
        return count; // Return number of inversion
    }

    public static int mergeSort(long[] arr, int left, int right) {
        int count = 0; // Count Total number of Inversion
        if (left >= right) return count;

        int mid = (left + right) / 2;
        count += mergeSort(arr, left, mid);
        count += mergeSort(arr, mid + 1, right);
        count += merge(arr, left, mid, right);

        return count;
    }

    public static long getInversions(long arr[], int n) {
        return mergeSort(arr, 0, n - 1);
    }
}
```
### Complexity Analysis
- **Time Complexity**: `O(n log n)`  
  - The merge sort algorithm divides the array into two halves recursively, and the merge step takes `O(n)` time. The total time complexity is `O(n log n)`.
- **Space Complexity**: `O(n)`  
  - A temporary array of size `n` is used for merging.
