# Inorder Predecessor of Binary Search Tree

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| **Medium** | BST   |           | [Video](https://youtu.be/SXKAD2svfmI)   |
|            |       |           | [Article](https://www.geeksforgeeks.org/inorder-predecessor-in-binary-search-tree/) |

## Description
Given a Binary Search Tree, the task is to find the In-Order predecessor of a given target key.

In a Binary Search Tree (BST), the Inorder predecessor of a node is the previous node in the Inorder traversal of the BST. The Inorder predecessor is NULL for the first node in the Inorder traversal.

**Note** : Predecessor means the node or node's value right before the target node or target node's value.

**Examples**
```
Input: root = [20, 8, 22, 4, 12, N, N, N, N, 10, 14], k = 8

             20
            /   \
           8     22
          / \
         4   12
            /  \
           10   14

Output: 4
Explanation: Inorder traversal: 4 8 10 12 14 20 22. Hence, predecessor of 8 is 4.
```

**Constraints**

- 1 <= no. of nodes <= 105 
- 1 <= node->data <= 106
- 1 <= key <= 106

## [Naive Approach] Using Inorder Traversal - O(n) time & O(n) space

### Code
```java
class Solution {
    public int findInorderPredecessor(Node root, Node targetNode) {
        List<Integer> inorderTraversal = new ArrayList<>(); // inorder traversal of the tree
        traverseInorder(root, inorderTraversal);
        
        int predecessor = -1;
        
        for (int i = 0; i < inorderTraversal.size(); i++) {
            if (inorderTraversal.get(i) < targetNode.data) {
                predecessor = inorderTraversal.get(i);
            } else {
                break;
            }
        }
        return predecessor;
    }
    
    private void traverseInorder(Node root, List<Integer> inorderTraversal) {
        if (root == null) {
            return;
        }
        
        // left root right
        traverseInorder(root.left, inorderTraversal);
        inorderTraversal.add(root.data);
        traverseInorder(root.right, inorderTraversal);
    }
}
```

### Complexity Analysis

- **Time Complexity** - `O(n) + O(n)`
- **Space Complexity** - `O(n)` 


## Using BST Search – O(h) Time and O(1) Space

### Algorithm
1. declare a variable `predecessor` with value `-1`. 
2. Start iterating on the BST, until we reach to `null`, and at each iteration will check
   1. if(root.data < target.data) 
      1. store the root's data to the predecessor.
      2. move to right. since we are not sure whether this node is the right before element of target or not.
   2. if(root.data >= target.data)
      1. Move to left, since we are looking for element smaller the target.
3. return the predecessor once we reach to `null`.

### Code
```java
class Solution {
    public int inorderPredecessor(Node root, Node targetNode) {
        // stores the node or node's data right before the target node or target node's data
        int predecessor = -1;
        
        // iterate on the binary search tree until we reach null
        while (root != null) {
            // if the current node's data is less than the target node's data
            if (root.data < targetNode.data) {
                // store the current node's data as the predecessor
                predecessor = root.data;
                // move to the right subtree
                root = root.right;
            } 
            // if the current node's data is greater than or equal to the target node's data
            else if (root.data >= targetNode.data) {
                // move to the left subtree
                root = root.left;
            }
        }
        // return the predecessor
        return predecessor;
    }
}
```

### Complexity Analysis

- **Time Complexity** - `O(h)`, `h` is the height of the binary tree. in the worst case `h` can be `n` if its a squed tree.
- **Space Complexity** - `O(1)` 
