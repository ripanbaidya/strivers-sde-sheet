# [Floor in BST](https://www.naukri.com/code360/problems/floor-from-bst_920457?source=youtube&campaign=Striver_Tree_Videos&utm_source=youtube&utm_medium=affiliate&utm_campaign=Striver_Tree_Videos&leftPanelTabValue=PROBLEM)

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| **Medium** | BST   |           | [Video](https://youtu.be/xm_W1ub-K-w)   |
|            |       |           | [Article](https://www.geeksforgeeks.org/floor-in-binary-search-tree-bst/) |

## Description
You are given a BST(Binary Search Tree) with n number of nodes and value x. your task is to find the greatest value node of the BST which is smaller than or equal to x.
Note: when x is smaller than the smallest node of BST then returns -1.

**floor** : Value which is smaller than equals key `(val <= key)` but largest among all the numbers given.

**Examples**

```
Examples 1:
Input: n = 7
                    2
                     \
                      81
                    /     \
                 42       87
                   \       \
                    66      90
                   /
                 45
x = 87
Output: 87
Explanation: 87 is present in tree so floor will be 87.
```

```
Example 2:
Input: n = 4                    
                          6
                           \
                            8
                          /   \
                        7       9
x = 11
Output: 9
```

```
Example 3:
Input: n = 4                     
                           6
                           \
                            8
                          /   \
                        7       9
x = 5
Output: -1
```

**Constraint:**

- 1 <= Noda data <= 109
- 1 <= n <= 105



## [Expected Approach] Iterative Solution- O(h) Time and O(1) Space
 
### Code
```java
public class Solution {
    /**
     * Finds the floor of a given key in a BST.
     * The floor of a key is the greatest value in the BST less than or equal to the key.
     */
    public int findFloorInBST(TreeNode root, int key) {
        int floorValue = -1;

        while (root != null) {
            if (root.data == key) {
                return root.data;
            }

            if (root.data < key) {
                floorValue = root.data;
                // move to the right subtree if the current node's value is less than the key
                root = root.right; 
            } else {
                // move to the left subtree if the current node's value is greater than the key
                root = root.left; 
            }
        }
        return floorValue;
    }
}
```

### Complexity Analysis

- **Time Complexiy** : `O(n)`
- **Space Complexity** : `O(1)`

