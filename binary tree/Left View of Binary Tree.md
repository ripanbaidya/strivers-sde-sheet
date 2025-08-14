# [Left View of Binary Tree](https://www.geeksforgeeks.org/problems/left-view-of-binary-tree/1)

| Difficulty | Topic | Companies | Resources                                                         |
| ---------- | ----- | --------- | ----------------------------------------------------------------- |
| Easy       | Tree  | Amazon    | [Video](https://youtu.be/KV4mRzTjlAk)                             |
|            |       | Microsoft | [Article](https://www.geeksforgeeks.org/print-left-view-binary-tree/) |

## Description
Given a Binary Tree, the task is to print the left view of the Binary Tree. The left view of a Binary Tree is a set of leftmost nodes for every level.

![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/876845/Web/Other/blobid0_1731456264.png)

```
Input: root[] = [1, 2, 3, 4, 5, N, N]
Output: [1, 2, 4]
```

## [Approach – 1] Using Level Order Traversal (BFS) – O(n) Time and O(n) Space

### Code
```java
class Solution {
    ArrayList<Integer> leftView(Node root) {
        // left view
        ArrayList<Integer> ans = new ArrayList<>(); 
        Queue<Node> q = new LinkedList<>();
        
        if(root == null) return ans;
        q.offer(root);
        
        while(!q.isEmpty()){
            int length = q.size();
            
            for(int i = 0; i < length; i ++){
                Node curNode = q.poll();
                
                if(curNode.left != null) {
                    q.offer(curNode.left);
                }
                if(curNode.right != null) {
                    q.offer(curNode.right);
                }

                // The first element at each level is part of the left view
                if(i == 0) {
                    ans.add(curNode.data);
                }
            }
        }
        return ans;
    }
}
```

> **Follow-up**: To obtain the right view, select the last element from each level, as it represents the rightmost node visible from that perspective.

### Complexity Analysis

- **Time Complexity: `O(n)`** Since we visit each node once, the time complexity is O(n), where n is the number of nodes in the tree.
- **Space Complexity: `O(n)`** in the worst case.
