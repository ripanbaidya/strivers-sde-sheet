# [Kth largest element in BST](https://www.geeksforgeeks.org/problems/kth-largest-element-in-bst/1)

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| **Medium** | BST   |           | [Video](https://youtu.be/9TJYWh0adfk)   |
|            |       |           | [Article](https://www.geeksforgeeks.org/kth-largest-element-bst-using-constant-extra-space/) |

## Description
Given a Binary Search Tree. Your task is to complete the function which will return the kth largest element without doing any modification in the Binary Search Tree.

**Examples**

```
Example 1:
Input:
      4
    /   \
   2     9
k = 2 
Output: 4
Explanation: 2nd Largest element in BST is 4
```

```
Example 2:
Input:
       9
        \ 
          10
k = 1
Output: 10
Explanation: 1st Largest element in BST is 10
```

**Constraints:**

- 1 <= number of nodes <= 105
- 1 <= node->data <= 105
- 1 <= k <= number of nodes


## [Expected Approach] Using In-Order traversal – O(k) Time and O(h) Space 
 
### Code
```java
class Solution {
    /**
     * Finds the kth largest element in the binary search tree.
     */
    public int kthLargest(Node root, int k) {
        int[] count = new int[]{0}; // count of nodes visited
        int[] result = new int[1];  // result to store the answer
        
        // perform a depth-first search of the tree
        dfs(root, k, count, result);
        
        return result[0];
    }
    
    /**
     * Recursive helper function to traverse the tree and find the kth largest element.
     */
    private void dfs(Node root, int k, int[] count, int[] result) {
        if (root == null) {
            return;
        }
        
        // traverse the right subtree
        dfs(root.right, k, count, result);
        
        // count the number of nodes visited
        count[0]++;
        
        // if the count is equal to k, store the value of the node
        if (count[0] == k) {
            result[0] = root.data;
            return;
        }
        
        // traverse the left subtree
        dfs(root.left, k, count, result);
    }
}
```

### Complexity Analysis

- **Time Complexity** : `O(k)`
- **Space Complexity** : `O(h)`
