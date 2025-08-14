# Root to Node Path in Binary Tree

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| Easy       | Tree  |           | [Video](https://youtu.be/fmflMqVOC7k)   |
|            |       |           | [Article](https://www.geeksforgeeks.org/print-path-root-given-node-binary-tree/) |

**Question Variation**

1. You are given a binary tree with ‘N’ number of nodes and a node ‘X’. Your task is to print the path from the root node to the given node ‘X’. [[Practice]](https://www.naukri.com/code360/problems/path-in-a-tree_3843990?leftPanelTabValue=PROBLEM)
    
2. Given the root of a binary tree, return all root-to-leaf paths in any order.
A leaf is a node with no children. [[Practice]](https://leetcode.com/problems/binary-tree-paths/description/)

## Description
Given the root of a binary tree, return all root-to-leaf paths in any order.

A leaf is a node with no children.

![](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)
```
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
```

**Constraints:**

- The number of nodes in the tree is in the range [1, 100].
- -100 <= Node.val <= 100



## [Find Root to all node path] Variation 2 Solution Using Recursion

### Code
```java
class Solution {
    private void buildPaths(TreeNode root, StringBuilder path, List<String> result) {
        if (root == null) return;

        // Add the current node's value to the path
        int prevLength = path.length();
        path.append(root.val);

        // If the current node is a leaf node, add the path to the result list
        if (root.left == null && root.right == null) {
            result.add(path.toString());
        } else {
            // Recursively traverse the left and right subtrees
            path.append("->");
            buildPaths(root.left, path, result);
            buildPaths(root.right, path, result);
        }

        // Backtrack to the previous node
        path.setLength(prevLength);
    }

    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        buildPaths(root, new StringBuilder(), result);
        return result;
    }
}
```

### Complexity Analysis

- **Time Complexity** :
    - The function traverses the entire binary tree to find all root-to-leaf paths.
    - Each node is visited once, and the path is constructed for each leaf node.
    - Time Complexity: O(N) where N is the number of nodes in the binary tree.
  
- **Space Complexity** : 
    - The space complexity is determined by the recursion stack and the path string.
    - The recursion stack can go as deep as the height of the tree.
    - The path string is built for each leaf node, but since it is backtracked, it does not contribute to additional space usage.
    - Space Complexity: O(H) where H is the height of the tree.


## [Path in a Tree] Variation 1 Solution Using Recursion

### Code
```java
public class Solution {

    // Helper function to find path from root to the given node 'x'
    public static boolean getPath(TreeNode root, ArrayList<Integer> path, int x) {
        if (root == null) return false;

        // Add the current node's value to the path
        path.add(root.data);

        // Check if current node's value is the target value 'x'
        if (root.data == x) return true;

        // Recursively check left and right subtrees for the target value
        if (getPath(root.left, path, x) || getPath(root.right, path, x)) return true;

        // Backtrack: remove the current node's value from path if 'x' not found
        path.remove(path.size() - 1);

        return false;
    }

    // Main function to return the path from root to node 'x'
    public static ArrayList<Integer> pathInATree(TreeNode root, int x) {
        ArrayList<Integer> path = new ArrayList<>();
        if (root != null) {
            getPath(root, path, x);
        }
        return path;
    }
}
```

### Complexity Analysis

- **Time Complexity** :
    - The function traverses the binary tree in a depth-first manner.
    - In the worst case, it visits all nodes in the tree to find the target node x.
    - Time Complexity: O(N) where N is the number of nodes in the binary tree.
  
- **Space Complexity** : 
    - The space complexity is determined by the recursion stack and the path list.
    - In the worst case, the recursion stack can go as deep as the height of the tree.
    - The path list stores the nodes along the path from the root to the target node, which is also proportional to the height of the tree.
    - Space Complexity: O(H) where H is the height of the tree.