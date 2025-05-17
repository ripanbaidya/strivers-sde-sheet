# [Populate Next Right pointers of Tree](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/description/)

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| **Medium** | Tree  |           | [Video](#)  |
|            |       |           | [Article](https://www.geeksforgeeks.org/connect-nodes-at-same-level/) |

## Description
You are given a perfect binary tree where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```java
struct Node {
    int val;
    Node *left;
    Node *right;
    Node *next;
}
```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

**Examples**

<img src="https://assets.leetcode.com/uploads/2019/02/14/116_sample.png" height=200 width=450>

```
Example 1:
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

Example 2:
Input: root = []
Output: []
```

**Constraints:**

- The number of nodes in the tree is in the range [0, 212 - 1].
- -1000 <= Node.val <= 1000
 

**Follow-up:**

- You may only use constant extra space.
- The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.


## [Expected Approach – 1] Using Level Order Traversal – O(n) Time and O(n) Space

### Intution

This idea is to use level order traversal to connect nodes at the same level. A NULL is pushed after each level to track the end of the level. As nodes are processed, each node’s nextRight pointer is set to the next node in the queue. If a NULL is encountered and the queue isn’t empty, another NULL is added to mark the end of the next level. This ensures that all nodes at the same level are linked. Please refre to Connect Nodes at same Level (Level Order Traversal) for implementation.

### Code
```java
class Solution {
    public Node connect(Node root) {
        if(root == null) return root; // base case

        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);

        while(!queue.isEmpty()){
            int levelSize = queue.size(); // current length of the level

            Node previousNode = null;
            for(int i = 0; i < levelSize; i ++){
                Node currentNode = queue.poll(); // currentNode node

                if(currentNode.left != null) queue.offer(currentNode.left);
                if(currentNode.right != null) queue.offer(currentNode.right);
                
                if(previousNode != null) {
                    previousNode.next = currentNode; // connect the two nodes
                }

                previousNode = currentNode;
            }
            previousNode.next = null; // last node of level
        }
        return root;
    }
}
```

### Complexity Analysis

- **Time Complexity** - `O(n)`
- **Space Complexity** - `O(n)` 
