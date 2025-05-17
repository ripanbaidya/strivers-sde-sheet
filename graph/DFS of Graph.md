# [DFS of Graph](https://www.geeksforgeeks.org/problems/depth-first-traversal-for-a-graph/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | DFS, Graph   | Amazon              | [Video](https://www.youtube.com/watch?v=Qzf1a--rhp8&t=6s)   |
|            |              | Samsung, intuit     | [Article](https://www.geeksforgeeks.org/depth-first-search-or-dfs-for-a-graph/) |

### Description
Given a connected undirected graph containing V vertices represented by a 2-d adjacency list adj[][], where each adj[i] represents the list of vertices connected to vertex i. Perform a Depth First Search (DFS) traversal starting from vertex 0, visiting vertices from left to right as per the given adjacency list, and return a list containing the DFS traversal of the graph.

Note: Do traverse in the same order as they are in the given adjacency list.

**Examples**

![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700203/Web/Other/blobid0_1728647807.jpg)
```
Input: adj[][] = [[2, 3, 1], [0], [0, 4], [0], [2]]

Output: [0, 2, 4, 3, 1]
Explanation: Starting from 0, the DFS traversal proceeds as follows:
Visit 0 → Output: 0 
Visit 2 (the first neighbor of 0) → Output: 0, 2 
Visit 4 (the first neighbor of 2) → Output: 0, 2, 4 
Backtrack to 2, then backtrack to 0, and visit 3 → Output: 0, 2, 4, 3 
Finally, backtrack to 0 and visit 1 → Final Output: 0, 2, 4, 3, 1
```

![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700203/Web/Other/blobid1_1728648013.jpg)
```
Input: adj[][] = [[1, 2], [0, 2], [0, 1, 3, 4], [2], [2]]

Output: [0, 1, 2, 3, 4]
Explanation: Starting from 0, the DFS traversal proceeds as follows: 
Visit 0 → Output: 0 
Visit 1 (the first neighbor of 0) → Output: 0, 1 
Visit 2 (the first neighbor of 1) → Output: 0, 1, 2 
Visit 3 (the first neighbor of 2) → Output: 0, 1, 2, 3 
Backtrack to 2 and visit 4 → Final Output: 0, 1, 2, 3, 4
```

**Constraints:**
- 1 ≤ V = adj.size() ≤ 10^4
- 1 ≤ adj[i][j] ≤ 10^4

## DFS Traversal of Graph
To perform a Depth First Search (DFS) traversal on an undirected graph, we can start with any arbitrary node and maintain a visited array to keep track of the nodes that have already been visited. Then, for each node, we perform a DFS traversal on its adjacency list. The result of the traversal is stored in a list or vector.

### Algorithm
1. **Initialize**:
	- Create a `visited` array to keep track of visited nodes.
	- Create an `ArrayList` to store the DFS result.
2. **Call Helper Function**:
	- Pass the starting node to the helper function.
3. **Helper Function**:
	- Mark the current node as **visited**.
	- Add the current node to the **result** ArrayList.
	- Iterate through the **adjacent nodes** of the current node.
	- Recursively call the helper function for each adjacent node.
4. **Return Result**:
	- Return the completed DFS result ArrayList.

### Code
```java
class Solution {
    // Helper function to perform DFS recursively
    private void depthFirstSearch(int node, boolean[] visited, ArrayList<ArrayList<Integer>> adjacencyList, ArrayList<Integer> result) {
        visited[node] = true; // Mark the node as visited
        result.add(node); // Add the node to the DFS result

        // Traverse all the adjacent nodes of the current node
        for (int neighbor : adjacencyList.get(node)) {
            if (!visited[neighbor]) {
                depthFirstSearch(neighbor, visited, adjacencyList, result);
            }
        }
    }

    // Function to return Depth First Search (DFS) Traversal of a given graph
    public ArrayList<Integer> performDFS(ArrayList<ArrayList<Integer>> adjacencyList) {
        int numberOfVertices = adjacencyList.size();
        boolean[] visited = new boolean[numberOfVertices]; // Array to track visited nodes
        ArrayList<Integer> dfsResult = new ArrayList<>(); // List to store the DFS result

        // Start DFS traversal from the first node (node 0)
        depthFirstSearch(0, visited, adjacencyList, dfsResult);
        return dfsResult;
    }
}
```

### Complexity Analysis

- **Time Complexity**: For an undirected graph, O(N) + O(2E), For a directed graph, O(N) + O(E), Because for every node we are calling the recursive function once, the time taken is O(N) and `2*E` is for total degrees as we traverse for all adjacent nodes.

- **Space Complexity**: O(3N) ~ O(N), Space for dfs stack space, visited array and an adjacency list.
