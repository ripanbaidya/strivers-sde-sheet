# Maximum width of a Binary Tree

| Difficulty | Topic | Companies | Resources                                                             |
| ---------- | ----- | --------- | --------------------------------------------------------------------- |
| Easy       | Tree  | Amazon    | [Video](https://youtu.be/ZbybYvcVLks)                                 |
|            |       | Microsoft | [Article](https://www.geeksforgeeks.org/maximum-width-of-a-binary-tree/) |

**Question Variation**

1. Given a Binary Tree, find the maximum width of it. Maximum width is defined as the maximum number of nodes at any level. [[Practice]](https://www.geeksforgeeks.org/problems/maximum-width-of-tree/1?page=4&category=Tree,Binary%20Search%20Tree&sortBy=submissions)

1. [662. Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/description/)

## Description
Given the root of a binary tree, return the maximum width of the given tree.

The maximum width of a tree is the maximum width among all levels.

The width of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes that would be present in a complete binary tree extending down to that level are also counted into the length calculation.

It is guaranteed that the answer will in the range of a 32-bit signed integer.

**Examples**

![](https://assets.leetcode.com/uploads/2021/05/03/width1-tree.jpg)

```
Example 1:
Input: root = [1,3,2,5,3,null,9]
Output: 4
Explanation: The maximum width exists in the third level with length 4 (5,3,null,9).
```

![](https://assets.leetcode.com/uploads/2022/03/14/maximum-width-of-binary-tree-v3.jpg)

```
Example 2:
Input: root = [1,3,2,5,null,null,9,6,null,7]
Output: 7
Explanation: The maximum width exists in the fourth level with length 7 (6,null,null,null,null,null,7).
```

## [Expected Approach] Using BFS - GFG Solution

### Code
```java
class Solution {
    int getMaxWidth(Node root) {
        if(root == null) return 0;
        
        int maxiWidth = 0;
        Queue<Node> q = new LinkedList<>();
        q.offer(root);
        
        while(!q.isEmpty()){
            int length = q.size();
            
            for(int i = 0; i < length; i ++){
                Node current = q.poll();
                
                if(current.left != null) q.offer(current.left);
                if(current.right != null) q.offer(current.right);
            }
            maxiWidth = Math.max(length, maxiWidth);
        }
        return maxiWidth;
    }
}
```

### Complexity Analysis
- **Time complexity** : `O(n)`
- **Space complexity** : `O(n)`




## [Expected Approach] Using BFS - Leetcode Solution

### Code
```java
class Pair {
    TreeNode node;
    int index;  // index of node in the current level

    public Pair(TreeNode node, int index){
        this.node = node;
        this.index = index;
    }
}

class Solution {
    public int widthOfBinaryTree(TreeNode root) {
        if(root == null) return 0;

        Queue<Pair> q = new LinkedList<>();
        int maxWidth = 0;

        q.offer(new Pair(root, 0));

        while(!q.isEmpty()) {
            int size = q.size();
            int minIndex = q.peek().index; // minimum index
            int firstIndex = 0, lastIndex = 0;

            for(int i = 0; i < size; i++) {
                Pair current = q.poll();
                TreeNode curNode = current.node;
                int normalizedIndex = current.index - minIndex; // normalize index

                if(i == 0) firstIndex = normalizedIndex;
                if(i == size - 1) lastIndex = normalizedIndex;

                if(curNode.left != null)
                    q.offer(new Pair(curNode.left, 2 * normalizedIndex));
                if(curNode.right != null)
                    q.offer(new Pair(curNode.right, 2 * normalizedIndex + 1));
            }

            maxWidth = Math.max(maxWidth, lastIndex - firstIndex + 1);
        }

        return maxWidth;
    }
}
```

### Complexity Analysis
- **Time complexity** : `O(n)`
- **Space complexity** : `O(n)`

