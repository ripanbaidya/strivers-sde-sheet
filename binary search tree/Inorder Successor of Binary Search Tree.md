# Inorder Successor of Binary Search Tree

| Difficulty | Topic | Companies | Resources                                                                           |
| ---------- | ----- | --------- | ----------------------------------------------------------------------------------- |
| **Medium** | BST   |           | [Video](https://youtu.be/SXKAD2svfmI)                                               |
|            |       |           | [Article](https://www.geeksforgeeks.org/inorder-successor-in-binary-search-tree/) |

## Description
In the Binary Tree, the Inorder successor of a node is the next node in the Inorder traversal of the Binary Tree. Inorder Successor is NULL for the last node in Inorder traversal. 

**Note** : Successor means the node or node's value right after a target node or target node's value.

```
Input: root = [20, 8, 22, 4, 12, N, N, N, N, 10, 14], k = 8

             20
            /   \
           8     22
          / \
         4   12
            /  \
           10   14
Output: 10
Explanation: Inorder traversal: 4 8 10 12 14 20 22. Hence, successor of 8 is 10.
```


**Constraints**

- 1 <= no. of nodes <= 105 
- 1 <= node->data <= 106
- 1 <= key <= 106

## [Naive Approach] Using Inorder Traversal - O(n) time & O(n) space

### Code
```java
class Solution {
    public void inorderTraversal(Node currentNode, List<Integer> inorderList) {
        if (currentNode == null) return;
        
        // Traverse the left subtree
        inorderTraversal(currentNode.left, inorderList);
        // Visit the root node
        inorderList.add(currentNode.data);
        // Traverse the right subtree
        inorderTraversal(currentNode.right, inorderList);
    }

    public int findInorderSuccessor(Node root, Node targetNode) {
        List<Integer> inorderList = new ArrayList<>(); // List to store inorder traversal
        inorderTraversal(root, inorderList);
        int successorValue = -1;
        
        for (int value : inorderList) {
            if (value > targetNode.data) {
                successorValue = value;
                break;
            }
        }
        return successorValue;
    }
}
```

### Complexity Analysis

- **Time Complexity** - `O(n) + O(n)`
- **Space Complexity** - `O(n)` 


## Using BST Search – O(h) Time and O(1) Space

### Algorithm
1. declare a variable `successor` with value `-1`. 
2. Start iterating on the BST, until we reach to `null`, and at each iteration will check
   1. if(root.data > target.data) 
      1. store the root's data to the successor.
      2. move to left. since we are not sure whether this node is the right after element of target or not.
   2. if(root.data <= target.data)
      1. Move to right, since we are looking for element greater the target.
3. return the successor once we reach to `null`.

### Code
```java
class Solution {
    public int inorderSuccessor(Node root, Node targetNode) {
        int successor = -1; // the inorder successor of the target node
        
        // iterate until we reach null
        while (root != null) {
            // if the current node's data is greater than the target node's data
            if (root.data > targetNode.data) {
                // store the current node's data as the successor
                successor = root.data;
                // move to the left subtree
                root = root.left;
            } 
            // if the current node's data is less than or equal to the target node's data
            else if (root.data <= targetNode.data) {
                // move to the right subtree
                root = root.right;
            }
        }
        // return the successor
        return successor;
    }
}
```

### Complexity Analysis

- **Time Complexity** - `O(h)`, `h` is the height of the binary tree. in the worst case `h` can be `n` if its a squed tree.
- **Space Complexity** - `O(1)` 

