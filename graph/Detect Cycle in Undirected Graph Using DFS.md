# [Undirected Graph Cycle Using DFS](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Graph        | Amazon, Microsoft   | [Video](https://youtu.be/zQ3zgFypzX4)   |
|            |              | Samsung, Intuit     | [Article](https://takeuforward.org/data-structure/detect-cycle-in-an-undirected-graph-using-dfs/)   |

## Description
Given an undirected graph with V vertices and E edges, represented as a 2D vector edges[][], where each entry edges[i] = [u, v] denotes an edge between vertices u and v, determine whether the graph contains a cycle or not.

**Examples:**

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/891735/Web/Other/blobid1_1743510240.jpg" height=200 width=200/>

```
Example 1:
Input: V = 4, E = 4, edges[][] = [[0, 1], [0, 2], [1, 2], [2, 3]]
Output: true
Explanation: 

1 -> 2 -> 0 -> 1 is a cycle.
```

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/891735/Web/Other/blobid2_1743510254.jpg" height=200 width=200/>

```
Example 2:
Input: V = 4, E = 3, edges[][] = [[0, 1], [1, 2], [2, 3]]
Output: false
Explanation: 
 
No cycle in the graph.
```

**Constraints:**

- 1 ≤ V ≤ 105
- 1 ≤ E = edges.size() ≤ 105

## Solution Using DFS Traversal

### Intution
The cycle in a graph starts from a node and ends at the same node. DFS is a traversal technique that involves the idea of recursion and backtracking. DFS goes in-depth, i.e., traverses all nodes by going ahead, and when there are no further nodes to traverse in the current path, then it backtracks on the same path and traverses other unvisited nodes. The intuition is that we start from a source and go in-depth, and reach any node that has been previously visited in the past; it means there's a cycle.  

### Algorithm
The algorithm steps are as follows:

* In the DFS function call make sure to store the parent data along with the source node, create a visited array, and initialize to 0. The parent is stored so that while checking for re-visited nodes, we don’t check for parents. 

* We run through all the unvisited adjacent nodes using an adjacency list and call the recursive dfs function. Also, mark the node as visited.

* If we come across a node that is already marked as visited and is not a parent node, then keep on returning true indicating the presence of a cycle; otherwise return false after all the adjacent nodes have been checked and we did not find a cycle.

**NOTE**: We can call it a cycle only if the already visited node is a non-parent node because we cannot say we came to a node that was previously the parent node. 

### Dry Run
<img src="https://lh4.googleusercontent.com/L55EB8izFqzvrZ5RNcbqNs3rxrxpP8QMZHsnuUEiiloq-cTn2CaJ-sxaPUqxIirnf5YYdvWPN2ZgTfrRDjHUhUhcNgTCt_BT5mk0Ou_Kmv4ZUxChfIyAzPIibdorml7pZC_zhXwmbVY_HtuR1Wjadc8" height=300 width=500/>

### Code
```java
class Solution {
    // Converts an edge list to an adjacency list
    private static List<List<Integer>> buildAdjacencyList(int numVertices, int[][] edges) {
        List<List<Integer>> adjacencyList = new ArrayList<>();

        for (int i = 0; i < numVertices; i++) {
            adjacencyList.add(new ArrayList<>());
        }

        for (int[] edge : edges) {
            int vertex1 = edge[0];
            int vertex2 = edge[1];

            adjacencyList.get(vertex1).add(vertex2);
            adjacencyList.get(vertex2).add(vertex1); // Undirected graph
        }

        return adjacencyList;
    }

    // Helper function to perform DFS and detect a cycle
    private boolean detectCycleDFS(int currentNode, int parentNode, List<List<Integer>> adjacencyList, boolean[] visited) {
        visited[currentNode] = true; // Mark the current node as visited

        for (int neighbor : adjacencyList.get(currentNode)) {
            if (!visited[neighbor]) {
                if (detectCycleDFS(neighbor, currentNode, adjacencyList, visited)) {
                    return true; // Cycle detected in DFS
                }
            } else if (neighbor != parentNode) {
                return true; // Cycle detected due to a back edge
            }
        }
        return false; // No cycle found in this path
    }

    // Checks if the graph contains a cycle
    public boolean hasCycle(int numVertices, int[][] edges) {
        List<List<Integer>> adjacencyList = buildAdjacencyList(numVertices, edges);
        boolean[] visited = new boolean[numVertices];

        // Check all components of the graph (it may be disconnected)
        for (int i = 0; i < numVertices; i++) {
            if (!visited[i]) {
                if (detectCycleDFS(i, -1, adjacencyList, visited)) {
                    return true; // Cycle detected
                }
            }
        }
        return false; // No cycle found
    }
}
```

### Complexity Analysis
- **Time Complexity:** `O(N) + O(2*E) + O(N)`, O(N) + O(2E) space will take by bfs travesal and O(N) space when there are disconnected graph

- **Space Complexity**: `O(N) + O(N) ~ O(N)`, for queue and visited array