# [Directed Graph Cycle](https://www.geeksforgeeks.org/problems/detect-cycle-in-a-directed-graph/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Graph        | Amazon, Microsoft   | [Video](https://youtu.be/uzVUw90ZFIg)   |
|            |              | Samsung, Intuit     | [Article](https://takeuforward.org/data-structure/detect-a-cycle-in-directed-graph-topological-sort-kahns-algorithm-g-23/)   |

## Description
Given a Directed Graph with V vertices (Numbered from 0 to V-1) and E edges, check whether it contains any cycle or not.
The graph is represented as a 2D vector edges[][], where each entry edges[i] = [u, v] denotes an edge from verticex u to v.

**Examples:**

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700218/Web/Other/blobid0_1744197297.jpg" height=200 width=200/>

```
Example 1:
Input: V = 4, edges[][] = [[0, 1], [0, 2], [1, 2], [2, 0], [2, 3]]
Output: true
Explanation: The diagram clearly shows a cycle 0 → 2 → 0
```

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700218/Web/Other/blobid1_1744197327.jpg" height=200 width=200/>

```
Example 2:
Input: V = 4, edges[][] = [[0, 1], [0, 2], [1, 2], [2, 3]
Output: false
Explanation: no cycle in the graph
```

**Constraints:**

- 1 ≤ V, E ≤ 105


## Solution: Using DFS

### Intution
The main intuition behind this problem is that if we are able to find a node that is already visited and is also present in the recursion stack, then there is a cycle in the graph. This is because in a directed graph, if we can reach a node from the starting node, then we can't reach that node again if there is no cycle. So, we can maintain two arrays one for visited node and other for visited path. A directed graph would be called cyclic when its has a node already visited at previous and the path has already visited at previous. with this we can easily solve this problem using dfs traversal.

### Approach
1. Convert the given graph from matrix format to adjacency list format.
2. Create two arrays, `vis` and `visPath`, to keep track of visited nodes and visited paths.
3. Run a loop to check all `n` nodes in the graph. If a node is not visited, call the `dfs` function to check for a cycle.
4. In the `dfs` function, mark the current node as visited and mark the `visPath` as visited. Then, iterate over all its neighbors. Two conditions might occur:
   1. If a neighbor is not visited, call the `dfs` function recursively to check for a cycle. If the `dfs` call returns true, then a cycle is found and return true.
   2. If a neighbor is already visited and is also present in the `visPath`, then a cycle is found and return true.
5. If a node is visited and no cycle is found, then mark the `visPath` as unvisited to prevent revisiting the same path.
6. If no cycle is found after checking all nodes, return false.

### Code
```java
class Solution {
    // Converts the given graph from matrix format to adjacency list format.
    private static List<List<Integer>> constructAdjacencyList(int numVertices, int[][] edges) {
        List<List<Integer>> adjacencyList = new ArrayList<>();

        for (int i = 0; i < numVertices; i++) {
            adjacencyList.add(new ArrayList<>());
        }

        for (int[] edge : edges) {
            int sourceVertex = edge[0];
            int destinationVertex = edge[1];

            adjacencyList.get(sourceVertex).add(destinationVertex);
        }

        return adjacencyList;
    }

    private static boolean hasCycleUsingDfs(int startVertex, List<List<Integer>> adjacencyList, boolean[] visited, boolean[] visitedPath) {
        visited[startVertex] = true;
        visitedPath[startVertex] = true;

        for (int neighbor : adjacencyList.get(startVertex)) {
            if (!visited[neighbor]) {
                if (hasCycleUsingDfs(neighbor, adjacencyList, visited, visitedPath)) {
                    return true;
                }
            } else if (visitedPath[neighbor]) {
                return true;
            }
        }

        visitedPath[startVertex] = false;
        return false;
    }

    public boolean isCyclic(int numVertices, int[][] edges) {
        // Convert the given graph from matrix format to adjacency list format
        List<List<Integer>> adjacencyList = constructAdjacencyList(numVertices, edges);

        // Visited array & visited Path
        boolean[] visited = new boolean[numVertices];
        boolean[] visitedPath = new boolean[numVertices];

        // Check all components (graph may be disconnected)
        for (int i = 0; i < numVertices; i++) {
            if (!visited[i]) {
                if (hasCycleUsingDfs(i, adjacencyList, visited, visitedPath)) {
                    return true; // Cycle found
                }
            }
        }

        return false; // No cycle in any component
    }
}
```

### Complexity Analysis
- **Time Complexity:** O(N) + O(2*E) + O(N), 
    O(N)+O(2E) space will take by bfs travesal and O(N) space when there are disconnected graph
    
- **Space Complexity**: O(N) + O(N) ~ O(N), for queue and visited array