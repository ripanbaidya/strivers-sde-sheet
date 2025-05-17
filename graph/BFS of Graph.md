# [BFS of Graph](https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | BFS, Graph   | Amazon              | [Video](https://www.youtube.com/watch?v=-tgVpUgsQ5k)   |
|            |              | Samsung, intuit     | [Article](https://www.geeksforgeeks.org/breadth-first-search-or-bfs-for-a-graph/) |

### Description
Given a connected undirected graph containing V vertices represented by a 2-d adjacency list adj[][], where each adj[i] represents the list of vertices connected to vertex i. Perform a Breadth First Search (BFS) traversal starting from vertex 0, visiting vertices from left to right as per the given adjacency list, and return a list containing the BFS traversal of the graph.

Note: Do traverse in the same order as they are in the given adjacency list.

**Examples**

![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700203/Web/Other/blobid0_1728647807.jpg)
```
Input: adj[][] = [[2, 3, 1], [0], [0, 4], [0], [2]]

Output: [0, 2, 4, 3, 1]
Explanation: Starting from 0, the DFS traversal proceeds as follows:
Visit 0 → Output: 0 
Visit 2 (first neighbor of 0) → Output: 0, 2 
Visit 3 (next neighbor of 0) → Output: 0, 2, 3 
Visit 1 (next neighbor of 0) → Output: 0, 2, 3, 
Visit 4 (neighbor of 2) → Final Output: 0, 2, 3, 1, 4
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


## BFS traversal of Graph
The given code implements a Breadth First Search (BFS) traversal on an undirected graph. The algorithm traverses the graph level by level, starting with an arbitrary node. It visits all the nodes in the given node's adjacency list, then visits all the nodes in the second level, and so on. The result of the traversal is stored in a list or vector. The boolean array `vis` is used to keep track of the visited nodes.

### Algorithm

1. **Initialize**:
	* Get the number of vertices (`v`) in the graph.
	* Create a `visited` boolean array to track visited nodes.
	* Create an `ArrayList` (`ans`) to store the BFS traversal result.
2. **Create a Queue**:
	* Create a queue (`q`) and add the starting node (0) to it.
	* Mark the starting node as **visited**.
3. **BFS Traversal**:
	* While the queue is not empty:
		+ Dequeue a node (`node`) from the queue.
		+ Add the node to the `ans` ArrayList.
		+ Iterate through the **adjacent nodes** of the current node:
			- If an adjacent node is not visited, mark it as **visited** and add it to the queue.
4. **Return Result**:
	* Return the completed BFS traversal result ArrayList (`ans`).

### Code
```java
class Solution {
    public List<Integer> bfs(List<List<Integer>> adjacencyList) {
        // Number of vertices in the graph
        int numVertices = adjacencyList.size();

        // boolean array to keep track of visited vertices
        boolean[] visited = new boolean[numVertices];
        
        // List to store the BFS traversal
        List<Integer> traversal = new ArrayList<>();
        
        // Queue for BFS, starting with vertex 0
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(0);
        visited[0] = true;
        
        // Perform BFS traversal
        while(!queue.isEmpty()){
            int vertex = queue.poll();
            traversal.add(vertex);
            
            // Traverse all the adjacent vertices of the current vertex
            for(int adjacentVertex : adjacencyList.get(vertex)){
                if(!visited[adjacentVertex]){
                    visited[adjacentVertex] = true;
                    queue.offer(adjacentVertex);
                }
            }
        }
        return traversal;
    }
}
```

### Complexity Analysis

- **Time Complexity**: For an undirected graph, O(N) + O(2E), For a directed graph, O(N) + O(E), Because for every node we are calling the recursive function once, the time taken is O(N) and `2*E` is for total degrees as we traverse for all adjacent nodes.

- **Space Complexity**: O(3N) ~ O(N), Space for dfs stack space, visited array and an adjacency list.
