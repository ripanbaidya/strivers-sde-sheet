# [Directed Graph Cycle](https://www.geeksforgeeks.org/problems/detect-cycle-in-a-directed-graph/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Graph        | Amazon, Microsoft   | [Video](https://youtu.be/iTBaI90lpDQ?si=o_4jmaMPobBhWUv7)   |
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


## Solution: Using BFS (Khan's Algorithm)

### Intution
The Intution here is that if the graph is DAG (Directed Acyclic Graph) then by using Khan's Algorithm we can get the Topological Sort of the graph which will be of the size of V. But if the Graph has any cycle then the Topological Sort will not be possible and the size of the Topological Sort will be less than V. Thus, if the size of the Topological Sort is equal to V then the graph has no cycle otherwise it has a cycle.

### Approach

1. Initialize an `indegree` array to store the indegree of each vertex.
2. Traverse all edges in the graph to calculate the indegree for each vertex.
3. Create a `Queue` and enqueue all vertices with an indegree of 0.
4. Initialize a `count` variable to track the number of vertices included in the topological sort.
5. While the queue is not empty:
   1. Dequeue a vertex and increase the `count` by 1.
   2. Iterate through all adjacent vertices (neighbors) of the dequeued vertex and decrease their indegree by 1.
   3. If any neighbor's indegree becomes 0, enqueue it.
6. After processing all vertices, check if `count` is equal to the total number of vertices `V`:
   - If `count == V`, the graph has no cycle and is acyclic, return `false`.
   - If `count < V`, the graph contains a cycle, return `true`.
   
### Code
```java
class Solution {
    // Converts an adjacency matrix to an adjacency list
    public List<List<Integer>> constructAdjacencyList(int V, int[][] edges){
        List<List<Integer>> adjacencyList = new ArrayList<>();

        for (int i = 0; i < V; i++) {
            adjacencyList.add(new ArrayList<>());
        }

        for (int[] edge : edges) {
            int sourceVertex = edge[0];
            int destinationVertex = edge[1];

            adjacencyList.get(sourceVertex).add(destinationVertex);
        }

        return adjacencyList;
    }

    /**
     * Checks if the graph contains a cycle
     */
    public boolean isCyclic(int V, int[][] edges) {
        List<List<Integer>> adjacencyList = constructAdjacencyList(V, edges);

        // Initialize an array to store the indegree of each vertex
        int[] indegree = new int[V];

        // Calculate the indegree of each vertex
        for(int i = 0; i < V; i ++){
            for(int j : adjacencyList.get(i)){
                indegree[j] ++;
            }
        }

        // Put all the elements with an indegree of 0 into the queue
        Queue<Integer> queue = new LinkedList<>();
        for(int i = 0; i < V; i ++){
            if(indegree[i] == 0){
                queue.offer(i);
            }
        }

        // Count the number of elements that would be part of the topological array
        int count = 0;
        while(!queue.isEmpty()){
            int node = queue.poll();
            count ++;

            // Decrease the indegree of each neighbor by 1
            for(int neighbor : adjacencyList.get(node)){
                indegree[neighbor] --;

                // If the indegree of any neighbor becomes 0, add it to the queue
                if(indegree[neighbor] == 0)
                    queue.offer(neighbor);
            }
        }

        // Return true if the graph has a cycle, false otherwise
        return count == V ? false : true;
    }
}
```

### Complexity Analysis
- **Time Complexity:** `O(N) + O(2 * E)` 
    O(N)+O(2E) space will take by bfs travesal and O(N) space when there are disconnected graph

- **Space Complexity**: `O(N)`