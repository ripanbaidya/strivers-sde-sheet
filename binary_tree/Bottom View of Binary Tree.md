# [Bottom View of Binary Tree](https://www.geeksforgeeks.org/problems/bottom-view-of-binary-tree/1?page=1&category=Tree,Binary%20Search%20Tree&sortBy=submissions)

| Difficulty | Topic | Companies | Resources                                                                                   |
| ---------- | ----- | --------- | ------------------------------------------------------------------------------------------- |
| Easy       | Tree  | Amazon    | [Video](https://youtu.be/0FtVY6I4pB8)                                                       |
|            |       | Microsoft | [Article](https://www.geeksforgeeks.org/bottom-view-binary-tree/)                           |

## Description
Given a binary tree, an array where elements represent the bottom view of the binary tree from left to right.

**Note**: If there are multiple bottom-most nodes for a horizontal distance from the root, then the latter one in the level traversal is considered.

**Example1:** The Green nodes represent the bottom view of below binary tree.

![](https://media.geeksforgeeks.org/wp-content/uploads/20241028122252261866/sum-of-nodes-in-bottom-view-of-binary-tree.webp)

## [Expected Approach – 1] Using level order traversal – O(nlogn) Time and O(n) Space

### Code 
```java
class Pair{
    int line; 
    Node node; 
    
    public Pair(int line, Node node){
        this.line = line;
        this.node = node;
    }
}
class Solution{
    public ArrayList <Integer> bottomView(Node root) {
        ArrayList <Integer> ans = new ArrayList<Integer>();
        if(root == null) return ans; 
        
        // This TreeMap stores the vertical line number and the last node's data encountered for that line
        // during level order traversal. The last node for each vertical line represents the bottom view.
        TreeMap<Integer, Integer> mp = new TreeMap<>(); // {LineNumber, Node's data}
        Queue<Pair> q = new LinkedList<>();
        
        q.offer(new Pair(0, root));

        while(!q.isEmpty()){
            int length = q.size();
            
            for(int i = 0; i < length; i ++){
                Pair current = q.poll();    
                Node curNode = current.node;
                int curLine = current.line;
                
                // When traversing left, the line number decreases by 1, whereas when traversing right, it increases by 1.
                if(curNode.left != null) q.offer(new Pair(curLine -1, curNode.left));
                if(curNode.right != null) q.offer(new Pair(curLine +1, curNode.right));
                
                // At each level, the last node encountered is the one that will contribute to the bottom view,
                // since it will overwrite any previous node's data in the map.
                mp.put(curLine, curNode.data);
            }
        }

        // Extract all the bottom view nodes from the treemap and add them to the result list in order.
        for(int v : mp.values()) {
            ans.add(v);
        }

        // bottom view
        return ans;  
    }
}
```

### Complexity Analysis
- **Time Complexity** : `O(n)` * `O(log n)` + `O(n)` = `O(n log n)`
- **Space Complexity** : `O(n)` 
  
