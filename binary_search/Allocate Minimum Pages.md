# [Allocate Minimum Pages](https://www.geeksforgeeks.org/problems/allocate-minimum-number-of-pages0937/1)

| Difficulty | Topic         | Companies | Resources   |
| ---------- | ------------- | --------- | ----------- |
| **Medium** | Binary Search |           | [Video](https://youtu.be/gYmWHvRHu-s) |
|            |               |           | [Article](https://www.geeksforgeeks.org/allocate-minimum-number-pages/) |

## Description
You are given an array `arr[]` of integers, where each element `arr[i]` represents the number of pages in the `i-th` book. You are also given an integer `k` representing the number of students. The task is to allocate books to the students under the following constraints:

1. Each student must receive at least one book.
2. Each student must be assigned a contiguous sequence of books.
3. No book can be assigned to more than one student.

The objective is to minimize the maximum number of pages assigned to any student. In other words, among all possible valid allocations, find the arrangement where the student who receives the most pages has the smallest possible maximum.

**Note:** Return `-1` if a valid assignment is not possible.

**Examples:**
```
Example 1:
Input: arr[] = [12, 34, 67, 90], k = 2
Output: 113
Explanation: Allocation can be done in following ways:
[12] and [34, 67, 90] Maximum Pages = 191
[12, 34] and [67, 90] Maximum Pages = 157
[12, 34, 67] and [90] Maximum Pages = 113.
Therefore, the minimum of these cases is 113, which is selected as the output.
```

```
Example 2:
Input: arr[] = [15, 17, 20], k = 5
Output: -1
Explanation: Allocation can not be done.
```

```
Example 3:
Input: arr[] = [22, 23, 67], k = 1
Output: 112
```

**Constraints:**
- `1 <= arr.size() <= 10^6`
- `1 <= arr[i] <= 10^3`
- `1 <= k <= 10^3`


## [Expected Approach] Using Binary Search

### Code
```java
class Solution {
    private static int allocatedStudents(int[] arr, int pages){
        // conunt of student, who has allocated pages
        int std = 1; 
        int total = 0; 
        
        for(int i = 0; i < arr.length; i ++){
            total += arr[i];
            
            if(total > pages){
                std ++; // new student allocated
                total = arr[i]; // start new allocation
            }
        }
        return std; 
    }
    public static int findPages(int[] arr, int k) {
        if(k > arr.length) return -1;
        
        int low = 0; // maximum among all pages
        int high = 0; // sum of all pages. 
        int ans = -1; // minimum among all maximums
        
        // calculating maxi & sum of all for low & high
        for(int i = 0; i < arr.length; i ++){
            low = Math.max(low, arr[i]); // find maximum pages
            high += arr[i];
        }
        
        while(low <= high){
            int mid = (low+high)/2; // page want to allocate
            
            int cntStudent = allocatedStudents(arr, mid); // arr and pages 
            
            if(cntStudent <= k){
                ans = mid; 
                high = mid -1;
            } else {
                low = mid + 1;
            }
        }
        return ans;
    }
}
```

### Complexity Analysis

- **Time Complexity** : `O(n) + O(log d) * O(n)`, where `d` is `high - low`
- **Space Complexity** : `O(1)`