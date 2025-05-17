# [Preorder inorder postorder in a single traversal](https://www.naukri.com/code360/problems/tree-traversal_981269)

| Difficulty | Topic | Companies | Resources                                     |
| ---------- | ----- | --------- | --------------------------------------------- |
|            | Tree  |           | [Video](https://youtu.be/ySp2epYvgTE)         |
|            |       |           | [Article](https://www.geeksforgeeks.org/preorder-postorder-and-inorder-traversal-of-a-binary-tree-using-a-single-stack/) |

## Description
You have been given a Binary Tree of 'N'nodes, where the nodes have integer values.

Your task is to return the ln-Order, Pre-Order, and Post-Order traversals of the given binary tree.

**Examples**

![](https://files.codingninjas.in/tt1-6639.jpg)

```
For the given binary tree:
The Inorder traversal will be [5, 3, 2, 1, 7, 4, 6].
The Preorder traversal will be [1, 3, 5, 2, 4, 7, 6].
The Postorder traversal will be [5, 2, 3, 7, 6, 4, 1].
```

## Preorder, Postorder and Inorder Traversal of a Binary Tree using a single Stack

1. If the state is ‘1’ ie. preorder: store the node’s data in the preorder array and move its state to 2 (inorder) for this node. Push this updated state back onto the stack and push its left child as well.
2. If the state is ‘2’ ie. inorder: store the node’s data is the inorder array and update its state to 3 (postorder) for this node. Push the updated state back onto the stack and push the right child onto the stack as well.
3. If the state is ‘3’ ie. postorder: store the node’s data in the postorder array and pop it.
   
### Code
```java
class VisitedNode {
    int visitCount; // number of times, a node has been visited
    TreeNode node;

    public VisitedNode(int visitCount, TreeNode node) {
        this.visitCount = visitCount;
        this.node = node;
    }
}

public class Solution {
    public static List<List<Integer>> getTreeTraversal(TreeNode root) {
        List<Integer> inOrder = new ArrayList<>();
        List<Integer> preOrder = new ArrayList<>();
        List<Integer> postOrder = new ArrayList<>();
        Stack<VisitedNode> stack = new Stack<>();

        stack.push(new VisitedNode(1, root));

        while (!stack.isEmpty()) {
            VisitedNode current = stack.pop();
            int curVisitCount = current.visitCount;
            TreeNode curNode = current.node;

            // Preorder
            if (curVisitCount == 1) {
                preOrder.add(curNode.data);
                stack.push(new VisitedNode(2, curNode));
                if (curNode.left != null) {
                    stack.push(new VisitedNode(1, curNode.left));
                }
            }
            // Inorder
            else if (curVisitCount == 2) {
                inOrder.add(curNode.data);
                stack.push(new VisitedNode(3, curNode));
                if (curNode.right != null) {
                    stack.push(new VisitedNode(1, curNode.right));
                }
            }
            // Postorder
            else {
                postOrder.add(curNode.data);
            }
        }

        return Arrays.asList(preOrder, inOrder, postOrder);
    }
}
```

### Complexity Analysis

- **Time Complexity** : `O(n)`
- **Space Complexity** : `O(n)` 
  