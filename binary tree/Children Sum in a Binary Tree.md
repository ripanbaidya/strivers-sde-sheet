# [Children Sum in a Binary Tree](http://geeksforgeeks.org/problems/children-sum-parent/1)

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| **Medium** | Tree  |           | [Video](https://youtu.be/fnmisPM6cVo)   |
|            |       |           | [Article](https://www.geeksforgeeks.org/check-for-children-sum-property-in-a-binary-tree/) |

## Description
Given a binary tree having n nodes. Check whether all of its nodes have a value equal to the sum of their child nodes. Return 1 if all the nodes in the tree satisfy the given properties, else it returns 0. For every node, the data value must be equal to the sum of the data values in the left and right children. Consider the data value 0 for a NULL child. Also, leaves are considered to follow the property.

Examples:

**Examples**

```
Example 1:
Input:
Binary tree
       35
      /  \
     20   15
    / \   / \
   15  5 10  5

Output: 1
Explanation: 
Here, every node is sum of its left and right child.
```
```
Example 2:
Input:
Binary tree
       1
     /   \
    4     3
   /  
  5    
Output: 0
Explanation: 
Here, 1 is the root node and 4, 3 are its child nodes. 4 + 3 = 7 which is not equal to the value of root node. Hence, this tree does not satisfy the given condition.
```

**Constraints:**
- 1 <= number of nodes <= 105
- 0 <= node->data <= 105


## [Expected Approach] Using Queue – O(n) Time and O(n) Space

### Intution 
The idea is to traverse the tree using level order approach. For each node, check if it satisfies children sum property. If it does, then push its children nodes into the queue. Otherwise return 0.

### Code
```java
class Solution {
    // Returns 1 if children sum property holds for the given node and both of its children
    int isChildrenSumPropertyHeld(Node root) {
        // If root is NULL then return true
        if (root == null) return 1;

        Queue<Node> nodeQueue = new LinkedList<>();
        nodeQueue.add(root);

        while (!nodeQueue.isEmpty()) {
            Node currentNode = nodeQueue.poll();

            // if this node is a leaf node, then continue
            if (currentNode.left == null && currentNode.right == null) continue;

            int childrenSum = 0;

            // If left child is not present then 0
            // is used as data of left child
            if (currentNode.left != null) {
                childrenSum += currentNode.left.data;
            }

            // If right child is not present then 0
            // is used as data of right child
            if (currentNode.right != null) {
                childrenSum += currentNode.right.data;
            }

            // if current node does not follow the property, then return 0.
            if (currentNode.data != childrenSum)
                return 0;

            // Push the left child node
            if (currentNode.left != null) nodeQueue.add(currentNode.left);

            // Push the right child node
            if (currentNode.right != null) nodeQueue.add(currentNode.right);
        }
        
        return 1;
    }
}
```

### Complexity Analysis
- **time complexity** : `O(n)`, where n is the number of nodes in the tree.
- **space complexity** : `O(n)`