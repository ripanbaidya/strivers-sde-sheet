# [Ceil from BST](https://www.naukri.com/code360/problems/ceil-from-bst_920464?leftPanelTabValue=PROBLEM)

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| **Medium** | BST   |           | [Video](https://youtu.be/KSsk8AhdOZA)   |
|            |       |           | [Article](https://www.geeksforgeeks.org/floor-and-ceil-from-a-bst/) |

## Description
Given a BST and a number X, find Ceil of X.
Note: Ceil(X) is a number that is either equal to X or is immediately greater than X.

If Ceil could not be found, return -1.

**Ceil** : Value which is greater than equals key `(val >= key)` but smallest among all the numbers given.

**Examples**

```
Example 1:
Input: root = [5, 1, 7, N, 2, N, N, N, 3], X = 3
      5
    /   \
   1     7
    \
     2 
      \
       3
Output: 3
Explanation: We find 3 in BST, so ceil of 3 is 3.
```

```
Example 2:
Input: root = [10, 5, 11, 4, 7, N, N, N, N, N, 8], X = 6
     10
    /  \
   5    11
  / \ 
 4   7
      \
       8
Output: 7
Explanation: We find 7 in BST, so ceil of 6 is 7.
```

**Your task:**
You don't need to read input or print anything. Just complete the function findCeil() to implement ceil in BST which returns the ceil of X in the given BST.

**Constraints:**
- 1 <= Number of nodes <= 105
- 1 <= Value of nodes<= 105


## Ceil in Binary Search Tree using Recursion 

### Code
```java
public class Solution {
    /**
     * Finds the smallest element in the binary search tree that is greater than or equal to the given target value.
     */
    public static int findCeil(TreeNode<Integer> root, int target) {
        int ceil = -1;

        while (root != null) {
            if (root.data == target) {
                return root.data;
            }

            if (target < root.data) {
                ceil = root.data;
                root = root.left; // smaller elements are on the left
            } else {
                root = root.right;
            }
        }

        return ceil;
    }
}
```

### Complexity Analysis

- **Time Complexiy** : `O(n)`
- **Space Complexity** : `O(1)`

 