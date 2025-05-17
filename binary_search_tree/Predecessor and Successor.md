# [Predecessor and Successor](https://www.geeksforgeeks.org/problems/predecessor-and-successor/1?page=2&category=Tree,Binary%20Search%20Tree&sortBy=submissions)

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| **Medium** | BST   |           | [Video](https://youtu.be/SXKAD2svfmI)   |
|            |       |           | [Article](https://www.geeksforgeeks.org/inorder-predecessor-successor-given-key-bst/) |

## Description

You are given root node of the BST and an integer key. You need to find the in-order successor and predecessor of the given key. If either predecessor or successor is not found, then set it to NULL.

Note:- In an inorder traversal the number just smaller than the target is the predecessor and the number just greater than the target is the successor. 

**Examples**

![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700614/Web/Other/blobid4_1746526041.webp)

```
Example 1:
Input: root[] = [8, 1, 9, N, 4, N, 10, 3, N, N, N], key = 8
Output: 4 9
Explanation: In the given BST the inorder predecessor of 8 is 4 and inorder successor of 8 is 9.

Example 2:
Input: root[] = [10, 2, 11, 1, 5, N, N, N, N, 3, 6, N, 4, N, N], key = 11
Output: 10 -1
Explanation: In given BST, the inorder predecessor of 11 is 10 whereas it does not have any inorder successor.
```

**Constraints:**

- 1 <= no. of nodes <= 105 
- 1 <= node->data <= 106
- 1 <= key <= 106


## Using BST Search – O(h) Time and O(1) Space

### Code
```java
class Solution {
    // Finds the inorder successor of a given node in a binary search tree.
    public static Node findInorderSuccessor(Node root, int value) {
        Node successor = null;
        
        while(root != null){ // O(n)
            if(root.data > value){
                successor = root;
                root = root.left;
            } else { // (root.data <= value)
                root = root.right;
            }
        }
        return successor;
    }
    
    //Finds the inorder predecessor of a given node in a binary search tree.
    public static Node findInorderPredecessor(Node root, int value) {
        Node predecessor = null;
        
        while(root != null){ // O(n)
            if(root.data < value){
                predecessor = root;
                root = root.right;
            } else { // (root.data >= value)
                root = root.left;
            }
        }
        return predecessor;
    }
    
    // Finds the inorder predecessor and inorder successor of a given node in a binary search tree.
    public static void findPreSuc(Node root, Node[] pre, Node[] suc, int value) {
        suc[0] = findInorderSuccessor(root, value);
        pre[0] = findInorderPredecessor(root, value);
    }
}
```

### Complexity Analysis

- **Time Complexity** - `O(n) + O(n)` ~ `O(n)`
  - each O(n) for getting successor and predecessor
- **Space Complexity** - `O(1)` 
