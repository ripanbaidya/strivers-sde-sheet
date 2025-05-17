# [Check if Binary Tree is the mirror of itself or not](https://www.geeksforgeeks.org/symmetric-tree-tree-which-is-mirror-image-of-itself/)


| Difficulty | Topic | Companies | Resources                                                                                     |
| ---------- | ----- | --------- | --------------------------------------------------------------------------------------------- |
| **Medium** | Tree  |           | [Video](https://youtu.be/nKggNAiEpBE?si=RSh48PFPv9hZUIFH)                                     |
|            |       |           | [Article](https://www.geeksforgeeks.org/symmetric-tree-tree-which-is-mirror-image-of-itself/) |

## Description

Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

**Examples**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

```
Example 1:
Input: root = [1,2,2,3,4,4,3]
Output: true
```
![](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

```
Example 2:
Input: root = [1,2,2,null,3,null,3]
Output: false
```

**Constraints:**
- The number of nodes in the tree is in the range [1, 1000].
- -100 <= Node.val <= 100
 

**Follow up:** Could you solve it both recursively and iteratively?


## [Approach – 1] Using Recursion – O(n) Time and O(h) Space
The idea is to recursively compare the left and right subtrees of the root. For the tree to be symmetric, the root values of the left and right subtrees must match, and their corresponding children must also be mirrors. 

### Code
```java
class Solution {
    private boolean areTreesIdentical(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true; // both trees are null
        }
        if (left == null || right == null) {
            return false; // any one of them null, false
        }

        if (left.val != right.val) {
            return false; // not identical
        }

        return areTreesIdentical(left.left, right.right) && areTreesIdentical(left.right, right.left);
    }

    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true; // tree is empty
        }

        TreeNode left = root.left;
        TreeNode right = root.right;

        return areTreesIdentical(left, right);
    }
}
```

### Complexity Analysis

- **Time Complexiy** : `O(n)`
- **Space Complexity** : `O(h)`