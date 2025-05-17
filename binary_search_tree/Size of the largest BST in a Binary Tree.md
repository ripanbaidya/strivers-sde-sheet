# [Size of the largest BST in a Binary Tree](https://www.geeksforgeeks.org/problems/largest-bst/1)

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| **Medium** | BST   |           | [Video](https://youtu.be/X0oXMdtUDwo)   |
|            |       |           | [Article](https://www.geeksforgeeks.org/largest-bst-binary-tree-set-2/#naive-approach-using-recursion-on2-time-and-on-space)|

## Description
You're given a binary tree. Your task is to find the size of the largest subtree within this binary tree that also satisfies the properties of a Binary Search Tree (BST). The size of a subtree is defined as the number of nodes it contains.

Note: A subtree of the binary tree is considered a BST if for every node in that subtree, the left child is less than the node, and the right child is greater than the node, without any duplicate values in the subtree.

**Examples**

![](https://media.geeksforgeeks.org/wp-content/uploads/20241007154946544659/Root-to-leaf-path-sum-equal-to-a-given-number-copy.webp) ==>
![](https://media.geeksforgeeks.org/wp-content/uploads/20241008164418969970/Balance-a-Binary-Search-Tree-3-copy.webp)

```
Example 1:
Input: root = [5, 2, 4, 1, 3]
Output: 3
Explanation:The following sub-tree is a BST of size 3
```

**Constraints:**

- 1 ≤ number of nodes ≤ 105
- 1 ≤ node->data ≤ 105


## [Naive Approach] Using Recursion – O(n^2) Time and O(n) Space
The idea is simple, we traverse through the Binary tree (starting from root). For every node, we check if it is BST. If yes, then we return size of the subtree rooted with current node. Else, we recursively call for left and right subtrees and return the maximum of two calls.

Below is the implementation of the above approach.

### Code
```java
class Solution {
    /**
     * Checks if the binary tree is a valid BST within the given range.
     */
    static boolean isValidBST(Node node, int minValue, int maxValue) {
        if (node == null) {
            return true;
        }
        if (node.data <= minValue || node.data >= maxValue) {
            return false;
        }
        return isValidBST(node.left, minValue, node.data) &&
               isValidBST(node.right, node.data, maxValue);
    }

    /**
     * Calculates the size of the binary tree.
     */
    static int calculateSize(Node node) {
        if (node == null) {
            return 0;
        }
        return 1 + calculateSize(node.left) + calculateSize(node.right);
    }

    /**
     * Finds the size of the largest BST within the binary tree.
     */
    static int findLargestBST(Node root) {
        if (root == null) {
            return 0;
        }
        if (isValidBST(root, Integer.MIN_VALUE, Integer.MAX_VALUE)) {
            return calculateSize(root);
        }
        return Math.max(findLargestBST(root.left), findLargestBST(root.right));
    }
}
```

### Complexity Analysis

- **time complexity** : `O(n^2)`, where n is the number of nodes in the tree.
- **space complexity** : `O(n)`


## [Expected Approach] – Using Binary Search Tree Property – O(n) Time and O(n) Space

### Code
```java
class BstHelper { 
    private int minValue;
    private int maxValue;
    private int size; 
    
    public BstHelper(int minValue, int maxValue, int size){ 
        this.minValue = minValue;
        this.maxValue = maxValue;
        this.size = size;
    }
    
    public int getMinValue() {
        return minValue;
    }
    
    public int getMaxValue() {
        return maxValue;
    }
    
    public int getSize() {
        return size;
    }
}

class Solution {
    public static BstHelper largestBstBt(Node root) {
        // base case
        if (root == null) {
            return new BstHelper(Integer.MAX_VALUE, Integer.MIN_VALUE, 0);
        }
        
        BstHelper leftSubtree = largestBstBt(root.left);
        BstHelper rightSubtree = largestBstBt(root.right);
        
        // validating 
        if (root.data > leftSubtree.getMaxValue() && root.data < rightSubtree.getMinValue()) {
            return new BstHelper(Math.min(root.data, leftSubtree.getMinValue()), 
                                Math.max(root.data, rightSubtree.getMaxValue()), 
                                1 + leftSubtree.getSize() + rightSubtree.getSize());
        } else {
            return new BstHelper(Integer.MIN_VALUE, Integer.MAX_VALUE, Math.max(leftSubtree.getSize(), rightSubtree.getSize()));
        }
    }
    
    public static int largestBst(Node root) {
        return largestBstBt(root).getSize();
    }
}
```

### Complexity Analysis

- **time complexity** : `O(n)`, where n is the number of nodes in the tree.
- **space complexity** : `O(n)`
