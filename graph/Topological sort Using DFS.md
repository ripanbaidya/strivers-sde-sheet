# [Topological Sort Using DFS](https://www.geeksforgeeks.org/problems/topological-sort/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Graph        | Amazon              | [Video](https://youtu.be/5lZ0iJMrUMk?si=a15ixBdTJfR7ZonL)   |
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

**Constraints:**

- 2  ≤  V  ≤  103
- 1  ≤  E = edges.size()  ≤  (V * (V - 1)) / 2


## Solution Using DFS

### Intution
The goal of the problem is to find a topological sort of a Directed Acyclic Graph (DAG). Topological sorting is a linear ordering of vertices such that for every directed edge u -> v, vertex u appears before v in the ordering. The solution uses DFS (Depth First Search) to achieve this by leveraging the property that nodes should be added to the result in reverse order of their completion in DFS.

### Approach
1. Construct the Adjacency List: Create an adjacency list representation of the graph from the given edges.

2. Perform DFS:
* Use a visited array to keep track of visited nodes.
* For each unvisited node, perform a DFS traversal:
    * Visit all its neighbors recursively.
    * Once all neighbors are visited, push the current node onto a stack.

3. Extract Topological Order:
* After completing the DFS for all nodes, the stack will contain the nodes in reverse topological order.
* Pop elements from the stack and add them to the result list to get the correct topological order.

4. Return the Result: Return the list containing the topological order of the graph.
   
### Code
```java
class TopologicalSort {
    /**
     * Construct adjacency list from adjacency matrix
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
     * Perform depth-first search on the graph
     */
    public static void depthFirstSearch(int vertex, boolean[] visited, List<List<Integer>> adjacencyList, Stack<Integer> stack) {
        visited[vertex] = true;

        for (int neighbor : adjacencyList.get(vertex)) {
            if (!visited[neighbor]) {
                depthFirstSearch(neighbor, visited, adjacencyList, stack);
            }
        }
        stack.push(vertex);
    }

    /**
     * Perform topological sort on the graph using depth-first search
     */
    public static ArrayList<Integer> topologicalSort(int numVertices, int[][] edges) {
        List<List<Integer>> adjacencyList = constructAdjacencyList(numVertices, edges);
        Stack<Integer> stack = new Stack<>();
        boolean[] visited = new boolean[numVertices];

        for (int i = 0; i < numVertices; i++) {
            if (!visited[i]) {
                depthFirstSearch(i, visited, adjacencyList, stack);
            }
        }

        ArrayList<Integer> topologicalOrder = new ArrayList<>();

        while (!stack.isEmpty()) {
            topologicalOrder.add(stack.pop());
        }
        return topologicalOrder;
    }
}
```

### Complexity Analysis

- **Time Complexity** : `O(V) + O(2*E) + O(N)`
- **Space Complexity** : `O(N)`
