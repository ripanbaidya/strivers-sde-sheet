# [Top View of Binary Tree](https://www.geeksforgeeks.org/problems/top-view-of-binary-tree/1)

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| Medium     | Tree  |           | [Video](https://youtu.be/Et9OCDNvJ78)   |
|            |       |           | [Article](https://www.geeksforgeeks.org/print-nodes-top-view-binary-tree/) |

## Description
Given a binary tree. The task is to find the top view of the binary tree. The top view of a binary tree is the set of nodes visible when the tree is viewed from the top.

Note: 

Return the nodes from the leftmost node to the rightmost node.
If two nodes are at the same position (horizontal distance) and are outside the shadow of the tree, consider the leftmost node only. 

**Example 1: The Green colored nodes represents the top view in the below Binary tree.** 

![](https://media.geeksforgeeks.org/wp-content/uploads/20241223090309348746/top_view_example1.png)


## Using BFS – O(n * log n) Time and O(n) Space

### Intution

The **top view** of a binary tree means: if you look at the tree **from the top**, only the **first node at each vertical line** (or horizontal distance from root) is visible. 

- Imagine drawing vertical lines through the binary tree. 
- The **first node** you encounter (from top to bottom) on each vertical line is part of the top view.
- So we need a **level order traversal (BFS)**, because we want to process nodes level by level, and the first node we encounter at a particular horizontal distance is the top view node for that line.

### Algorithm

1. Initialize an empty list to store the top view of the binary tree.
2. Initialize a TreeMap, where the key is the horizontal distance (or line number) and the value is the node's value.
3. Initialize a Queue of Pair objects, where each Pair object contains a node and its corresponding line number.
4. Add the root node to the queue with line number 0.
5. While the queue is not empty, do the following:
   - Dequeue a node and its line number.
   - If the line number is not already in the TreeMap, add it with the node's value.
   - Add the left child of the node to the queue with line number - 1.
   - Add the right child of the node to the queue with line number + 1.
6. Return the list of node values from the TreeMap.

### Code
```java
class Pair{
    Node node;
    int line; // vertical line
    
    public Pair(Node node, int line){
        this.node = node;
        this.line = line;
    }
}

class Solution {
    static ArrayList<Integer> topView(Node root) {
        ArrayList<Integer> ans = new ArrayList<>();
        if(root == null) return ans; // base case
        
        // Store nodes to be processed in a queue
        Queue<Pair> q = new LinkedList<>();
        
        // This TreeMap will store the verticalLine number and for each line, the first element encountered during the
        // level order traversal, and that would be the top view of that level 
        Map<Integer, Integer> mp = new TreeMap<>(); // <lineNumber, Node's Data>
        
        // Start by pushing the root node with line number 0 into the queue.
        q.offer(new Pair(root, 0));
        
        while(!q.isEmpty()){
            int length = q.size();
            
            for(int i = 0; i < length; i ++){
                Pair current = q.poll();
                Node curNode = current.node;
                int curLine = current.line;
                
                // Store the first node encountered for each vertical line,
                // as it represents the top view from that line.
                mp.putIfAbsent(curLine, curNode.data);
                
                // Add its left child with line - 1
                if(curNode.left != null) q.offer(new Pair(curNode.left, curLine - 1));
                // Add its right child with line + 1
                if(curNode.right != null) q.offer(new Pair(curNode.right, curLine + 1));
            }
        }
        // Extract all the top view nodes from the treemap and add them to the result list in order.
        for(int value : mp.values()){
            ans.add(value);
        }
        
        // Returing the top view
        return ans; 
    }
}
```

### Complexity Analysis

- **Time Complexity**: `O(N log N)`
    - O(N) for level-order traversal (BFS).
    - O(log N) for inserting into the TreeMap for each node.
- **Space Complexity**: `O(N)`
    - O(N) for the TreeMap and Queue.
