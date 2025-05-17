# [Morris Inorder Traversal](https://www.geeksforgeeks.org/problems/inorder-traversal/1?page=2&category=Tree,Binary%20Search%20Tree&sortBy=submissions)

| Difficulty | Topic | Companies | Resources                                                                  |
| ---------- | ----- | --------- | -------------------------------------------------------------------------- |
| Easy       | Tree  | Amazon    | [Video](https://youtu.be/80Zug6D1_r4)                                      |
|            |       | Microsoft | [Article](https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion-and-without-stack/) |

## Description

Given the root of a binary tree, return the inorder traversal of its nodes' values.

<img src="https://assets.leetcode.com/uploads/2024/08/29/tree_2.png" height=200 width=250>

```
Example 1:
Input: root = [1,2,3,4,5,null,8,null,null,6,7,9]
Output: [4,2,6,5,7,1,3,9,8]
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

> The Morris Traversal is an in-order traversal of a binary tree without using a stack or recursion.
> And we have use this in this Implementation.

## Morris traversal for Inorder

### Code
```java
class Solution {
    ArrayList<Integer> inorderTraversal(Node root) {
        ArrayList<Integer> result = new ArrayList<>();
        Node currentNode = root;

        // we traverse all the nodes in the tree
        while (currentNode != null) {
            // if the node doesn't have a left child, we add it to the result list
            // and move to the right subtree
            if (currentNode.left == null) {
                result.add(currentNode.data);
                currentNode = currentNode.right;
            } else {
                // find the predecessor node
                Node predecessor = currentNode.left;
                // the predecessor node is the rightmost node in the left subtree
                while (predecessor.right != null && predecessor.right != currentNode) {
                    predecessor = predecessor.right;
                }
                // if the predecessor node is not pointing to the current node
                // we know we haven't visited the left subtree yet
                if (predecessor.right == null) {
                    // link the predecessor node to the current node
                    predecessor.right = currentNode;
                    // move to the left subtree
                    currentNode = currentNode.left;
                } else {
                    // if the predecessor node is already pointing to the current node
                    // we know we have visited the left subtree
                    // so we break the link and move to the right subtree
                    predecessor.right = null;
                    result.add(currentNode.data);
                    currentNode = currentNode.right;
                }
            }
        }
        return result;
    }
}
```

### Complexity Analysis

- **Time Complexity** : `O(n)`, if we take a closer look, we can notice that every edge of the tree is traversed at most three times.
- **Space Complexity** : `O(1)`, as we are using only constant variables.
  
