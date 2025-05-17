# [Undirected Graph Cycle Using BFS](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     |    Graph     | Amazon, Microsoft   | [Video](https://youtu.be/BPlrALf1LDU)   |
|            |              | Samsung, intuit     | [Article](https://takeuforward.org/data-structure/detect-cycle-in-an-undirected-graph-using-bfs/) |

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

## Solution Using BFS Traversal

### Intution
The intuition is that we start from a node, and start doing BFS level-wise, if somewhere down the line, we visit a single node twice, it means we came via two paths to end up at the same node. It implies there is a cycle in the graph because we know that we start from different directions but can arrive at the same node only if the graph is connected or contains a cycle, otherwise we would never come to the same node again.  

### Initial configuration:
* Queue: Define a queue and insert the source node along with parent data (<source node, parent>). For example, (2, 1) means 2 is the source node and 1 is its parent node.

* Visited array: an array initialized to 0 indicating unvisited nodes.  
The algorithm steps are as follows:

### Algorithm

* For BFS traversal, we need a queue data structure and a visited array. 

* Push the pair of the source node and its parent data (<source, parent>) in the queue, and mark the node as visited. The parent will be needed so that we don’t do a backward traversal in the graph, we just move frontwards. 

* Start the BFS traversal, pop out an element from the queue every time and travel to all its unvisited neighbors using an adjacency list.

* Repeat the steps either until the queue becomes empty, or a node appears to be already visited which is not the parent, even though we traveled in different directions during the traversal, indicating there is a cycle.

* If the queue becomes empty and no such node is found then there is no cycle in the graph.

### Dry Run
<img src="https://lh4.googleusercontent.com/L55EB8izFqzvrZ5RNcbqNs3rxrxpP8QMZHsnuUEiiloq-cTn2CaJ-sxaPUqxIirnf5YYdvWPN2ZgTfrRDjHUhUhcNgTCt_BT5mk0Ou_Kmv4ZUxChfIyAzPIibdorml7pZC_zhXwmbVY_HtuR1Wjadc8" height=300 width=500/>

### Code
```java
class Solution {
    /**
     * Converts a graph represented as an edge list to an adjacency list
     */
    private static List<List<Integer>> constructAdjacencyList(int numVertices, int[][] edges) {
        List<List<Integer>> adjacencyList = new ArrayList<>();

        for (int i = 0; i < numVertices; i++) {
            adjacencyList.add(new ArrayList<>());
        }

        for (int[] edge : edges) {
            int vertex1 = edge[0];
            int vertex2 = edge[1];

            adjacencyList.get(vertex1).add(vertex2);
            adjacencyList.get(vertex2).add(vertex1);
        }

        return adjacencyList;
    }

    /**
     * Checks if a graph contains a cycle using BFS
     */
    private boolean isCyclicUsingBFS(int startVertex, List<List<Integer>> adjacencyList, boolean[] visited) {
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{startVertex, -1});
        visited[startVertex] = true;

        while (!queue.isEmpty()) {
            int[] currentVertex = queue.poll();
            int vertex = currentVertex[0];
            int parent = currentVertex[1];

            for (int neighbor : adjacencyList.get(vertex)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(new int[]{neighbor, vertex});
                } else if (neighbor != parent) {
                    // if the neighbor is already visited and it is not the parent
                    // then a cycle is detected
                    return true;
                }
            }
        }

        return false;
    }

    /**
     * Checks if a graph contains a cycle
     */
    public boolean isCycle(int numVertices, int[][] edges) {
        List<List<Integer>> adjacencyList = constructAdjacencyList(numVertices, edges);
        boolean[] visited = new boolean[numVertices];

        // check all components (graph may be disconnected)
        for (int i = 0; i < numVertices; i++) {
            if (!visited[i]) {
                if (isCyclicUsingBFS(i, adjacencyList, visited)) {
                    return true; // cycle found
                }
            }
        }

        return false; // no cycle found
    }
}
```

### Complexity Analysis
- **Time Complexity:** `O(N) + O(2*E) + O(N)`, O(N) + O(2E) space will take by bfs travesal and O(N) space when there are disconnected graph

- **Space Complexity**: `O(N) + O(N) ~ O(N)`, for queue and visited array