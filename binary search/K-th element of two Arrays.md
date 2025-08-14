# [K-th element of two Arrays](https://www.geeksforgeeks.org/problems/k-th-element-of-two-sorted-array1317/1)

| Difficulty | Topic         | Companies | Resources   |
| ---------- | ------------- | --------- | ----------- |
| **Medium** | Binary Search |           | [Video]()   |
|            |               |           | [Article](https://www.geeksforgeeks.org/k-th-element-two-sorted-arrays/) |


## Description
Given two sorted arrays `a[]` and `b[]` and an integer `k`, the task is to find the element that would be at the k-th position (1-based indexing) of the combined sorted array formed by merging `a[]` and `b[]`.

**Examples**
```
Example 1:
Input: a[] = [2, 3, 6, 7, 9], b[] = [1, 4, 8, 10], k = 5
Output: 6
Explanation: The final combined sorted array would be [1, 2, 3, 4, 6, 7, 8, 9, 10]. The 5th element of this array is 6.

Example 2:
Input: a[] = [100, 112, 256, 349, 770], b[] = [72, 86, 113, 119, 265, 445, 892], k = 7
Output: 256
Explanation: Combined sorted array is [72, 86, 100, 112, 113, 119, 256, 265, 349, 445, 770, 892]. The 7th element of this array is 256.
```

**Constraints:**
- `1 <= a.size(), b.size() <= 10^6`
- `1 <= k <= a.size() + b.size()`
- `0 <= a[i], b[i] < 10^8`


## Using Optimized Merge of Merge Sort – O(k) Time and O(1) Space

### Intution
To find the k-th smallest element in the combined sorted array formed by merging two sorted arrays `a` and `b`, we can use a two-pointer technique. The idea is to traverse both arrays simultaneously, comparing elements at the current positions of the pointers, and moving the pointer pointing to the smaller element. We keep a count of the elements processed, and when the count reaches `k`, we return the current element.

### Code
```java
class Solution {
    public int findKthElement(int[] array1, int[] array2, int k) {
        int index1 = 0, index2 = 0;
        int length1 = array1.length, length2 = array2.length;
        int count = 0;
        int kthElement = -1;
        
        // Traverse both arrays and find the kth element
        while (index1 < length1 && index2 < length2) {
            if (array1[index1] <= array2[index2]) {
                kthElement = array1[index1++];
            } else {
                kthElement = array2[index2++];
            }
            count++;
            if (count == k) {
                return kthElement;
            }
        }
        
        // If elements remain in array1, continue traversal
        while (index1 < length1) {
            kthElement = array1[index1++];
            count++;
            if (count == k) {
                return kthElement;
            }
        }
        
        // If elements remain in array2, continue traversal
        while (index2 < length2) {
            kthElement = array2[index2++];
            count++;
            if (count == k) {
                return kthElement;
            }
        }
        
        return kthElement;
    }
}
```

### Complexity Analysis

- **Time Complexity**: `O(k)`, where `k` is the position of element
- **Space Complexity**: `O(1)`
