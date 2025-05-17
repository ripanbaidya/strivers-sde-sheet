# [Topological Sort Using BFS- Kahn's Algorithm](https://www.geeksforgeeks.org/problems/topological-sort/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Graph        | Amazon              | [Video](https://youtu.be/73sneFXuTEg?si=EcL9TetV_XwnG1tl)   |
|            |              | Samsung, intuit     | [Article](https://www.geeksforgeeks.org/topological-sorting/) |

## Description
Given a Directed Acyclic Graph (DAG) of V (0 to V-1) vertices and E edges represented as a 2D list of edges[][], where each entry edges[i] = [u, v] denotes a directed edge u -> v. Return the topological sort for the given graph.

Topological sorting for Directed Acyclic Graph (DAG) is a linear ordering of vertices such that for every directed edge u -> v, vertex u comes before v in the ordering.
Note: As there are multiple Topological orders possible, you may return any of them. If your returned Topological sort is correct then the output will be true else false.

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700255/Web/Other/blobid0_1744196747.jpg" height=250 width=300>

```
Examples 1:
Input: V = 4, E = 3, edges[][] = [[3, 0], [1, 0], [2, 0]]

Output: true
Explanation: The output true denotes that the order is valid. Few valid Topological orders for the given graph are:
[3, 2, 1, 0]
[1, 2, 3, 0]
[2, 3, 1, 0]
```

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700255/Web/Other/blobid1_1744196789.jpg" height=250 width=300>

```
Example 2:
Input: V = 6, E = 6, edges[][] = [[1, 3], [2, 3], [4, 1], [4, 0], [5, 0], [5,2]]

Output: true
Explanation: The output true denotes that the order is valid. Few valid Topological orders for the graph are:
[4, 5, 0, 1, 2, 3]
[5, 2, 4, 0, 1, 3]
```

```
Constraints:
2  ≤  V  ≤  103
1  ≤  E = edges.size()  ≤  (V * (V - 1)) / 2
```

## Solution Using BFS

### Intution
We can find The Topological sort of Graph Using BFS, is also knows as `Khan's Algorithms`. In this Algorithm, the intuition is to count the number of incoming edges of each node and whoever has the incoming edge count as 0, we consider them as the starting point of our traversal. We put all of them into the `queue` and run a while loop until the queue is not empty. Inside the while loop, we pop the element and add that element as a result. For all the nodes that are adjacent to the popped node, we decrement their incoming edge count. When a node's incoming edge count becomes 0, we add it to the queue. We keep running this process until the queue is empty.

### Approach

1. Declare an `indegree` array and count the indegree of each node.
2. Declare a `Queue` data structure and enqueue all elements with an `indegree` value of 0.
3. Create an `ArrayList` or `Vector` to store the result.
4. Run a while loop until the queue is empty, and inside the loop:
   1. Dequeue an element and add it to the resultant array.
   2. For each neighbor of the current node, which is essentially the outgoing node, decrement its value in the `indegree` array.
   3. If any node's `indegree` value becomes 0, enqueue that node.
   4. Repeat the process until the queue is empty.
5. Return the resultant array.
    
### Code
```java
class Solution {
    /**
     * Constructs an adjacency list from an adjacency matrix.
     */
    public static List<List<Integer>> constructAdjacencyList(int numVertices, int[][] edges) {
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

    /**
     * Performs topological sort on the graph using Khan's Algorithm.
     */
    public static List<Integer> topologicalSort(int numVertices, int[][] edges) {
        List<List<Integer>> adjacencyList = constructAdjacencyList(numVertices, edges);
        int[] inDegree = new int[numVertices];

        // Compute in-degree of each vertex
        for (int i = 0; i < numVertices; i++) {
            for (int neighbor : adjacencyList.get(i)) {
                inDegree[neighbor]++;
            }
        }

        // Queue for vertices with in-degree of 0
        Queue<Integer> zeroInDegreeQueue = new LinkedList<>();
        for (int i = 0; i < numVertices; i++) {
            if (inDegree[i] == 0) {
                zeroInDegreeQueue.offer(i);
            }
        }

        // List to store the topological order
        List<Integer> topologicalOrder = new ArrayList<>();

        while (!zeroInDegreeQueue.isEmpty()) {
            int currentVertex = zeroInDegreeQueue.poll();
            topologicalOrder.add(currentVertex);

            // Decrease in-degree of all adjacent vertices
            for (int neighbor : adjacencyList.get(currentVertex)) {
                inDegree[neighbor]--;

                // If in-degree becomes 0, add it to the queue
                if (inDegree[neighbor] == 0) {
                    zeroInDegreeQueue.offer(neighbor);
                }
            }
        }
        return topologicalOrder;
    }
}
```

### Complexity Analysis

- **Time Complexity** : `O(V) + O(2*E) + O(N)`
- **Space Complexity** : `O(N)`

