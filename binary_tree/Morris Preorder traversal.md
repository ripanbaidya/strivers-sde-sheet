# [Morris Preorder Traversal](https://www.geeksforgeeks.org/problems/preorder-traversal/1?page=2&category=Tree,Binary%20Search%20Tree&sortBy=submissions)

| Difficulty | Topic | Companies | Resources                                                                   |
| ---------- | ----- | --------- | --------------------------------------------------------------------------- |
| Easy       | Tree  | Amazon    | [Video](https://youtu.be/80Zug6D1_r4)                                       |
|            |       | Microsoft | [Article](https://www.geeksforgeeks.org/morris-traversal-for-preorder/) |

## Description

Given the root of a binary tree, return the preorder traversal of its nodes' values.

<img src="https://assets.leetcode.com/uploads/2024/08/29/tree_2.png" height=200 width=250>

```
Example 1:
Input: root = [1,2,3,4,5,null,8,null,null,6,7,9]
Output: [1,2,4,5,6,7,3,8,9]
```

```
Example 2:
Input: root = [1,null,2,3]
Output: [1,3,2]
```

**Constraints:**
- The number of nodes in the tree is in the range [0, 100].
- -100 <= Node.val <= 100
 

**Follow up:** Recursive solution is trivial, could you do it iteratively?

## Morris traversal for Preorder

### Code
```java
class Solution {
    ArrayList<Integer> preorder(Node root) {
        // result list to store node values
        ArrayList<Integer> result = new ArrayList<>();

        // current node
        Node currentNode = root;

        while (currentNode != null) {
            // if left child is null, add current node value to result and move to right subtree
            if (currentNode.left == null) {
                result.add(currentNode.data);
                currentNode = currentNode.right;
            } else {
                // find previous node in left subtree
                Node previousNode = currentNode.left;
                while (previousNode.right != null && previousNode.right != currentNode) {
                    previousNode = previousNode.right;
                }

                // if thread already exists, break it and move to right subtree
                if (previousNode.right != null) {
                    previousNode.right = null;
                    currentNode = currentNode.right;
                } 
                // if thread doesn't exist, create it and move to left subtree
                else {
                    previousNode.right = currentNode;
                    result.add(currentNode.data);
                    currentNode = currentNode.left;
                }
            }
        }

        return result;
    }
}
```

### Complexity Analysis
- **Time Complexity** : `O(n)`, we visit every node at most once.
- **Space Complexity** : `O(1)` 
  
