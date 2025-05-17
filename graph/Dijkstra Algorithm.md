# [Dijkstra Algorithm](https://www.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Graph        | Flipkart            | [Video](https://youtu.be/rp1SMw7HSO8)   |
|            | Algorithm    | Microsoft           | [Article](https://www.geeksforgeeks.org/dijkstras-shortest-path-algorithm-greedy-algo-7/) |

## Description
Given an undirected, weighted graph with V vertices numbered from 0 to V-1 and E edges, represented by 2d array edges[][], where edges[i]=[u, v, w] represents the edge between the nodes u and v having w edge weight.
You have to find the shortest distance of all the vertices from the source vertex src, and return an array of integers where the ith element denotes the shortest distance between ith node and source vertex src.

**Note**: The Graph is connected and doesn't contain any negative weight edge.

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/892538/Web/Other/blobid0_1744201836.jpg" height=250 width=330>

```
Input: V = 3, edges[][] = [[0, 1, 1], [1, 2, 3], [0, 2, 6]], src = 2
Output: [4, 3, 0]
Explanation:

Shortest Paths:
For 2 to 0 minimum distance will be 4. By following path 2 -> 1 -> 0
For 2 to 1 minimum distance will be 3. By following path 2 -> 1
For 2 to 2 minimum distance will be 0. By following path 2 -> 2
```

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/892538/Web/Other/blobid1_1744202046.jpg" height=250 width=330>

```
Input: V = 5, edges[][] = [[0, 1, 4], [0, 2, 8], [1, 4, 6], [2, 3, 2], [3, 4, 10]], src = 0
Output: [0, 4, 8, 10, 10]
Explanation: 

Shortest Paths: 
For 0 to 1 minimum distance will be 4. By following path 0 -> 1
For 0 to 2 minimum distance will be 8. By following path 0 -> 2
For 0 to 3 minimum distance will be 10. By following path 0 -> 2 -> 3 
For 0 to 4 minimum distance will be 10. By following path 0 -> 1 -> 4
```

**Constraints:**
* 1 ≤ V ≤ 105
* 1 ≤ E = edges.size() ≤ 105
* 0 ≤ edges[i][j] ≤ 104
* 0 ≤ src < V

### Articles
* <a href="https://www.geeksforgeeks.org/dijkstras-shortest-path-algorithm-greedy-algo-7/" target="_blank">GeeksforGeeks</a>
* <a href="https://takeuforward.org/data-structure/g-35-print-shortest-path-dijkstras-algorithm/" target="_blank">TakeUforward</a>
 
### Code
```java
class Solution {
    // Construct adjacency list using ArrayList of ArrayList
    public ArrayList<ArrayList<ArrayList<Integer>>> constructAdj(int[][] edges, int V) {
        // Initialize the adjacency list
        ArrayList<ArrayList<ArrayList<Integer>>> adj = new ArrayList<>();

        for(int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }

        // Fill the adjacency list from edges
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int wt = edge[2];

            // Add edge from u to v
            ArrayList<Integer> e1 = new ArrayList<>();
            e1.add(v);
            e1.add(wt);
            adj.get(u).add(e1);

            // Since the graph is undirected, add edge from v to u
            ArrayList<Integer> e2 = new ArrayList<>();
            e2.add(u);
            e2.add(wt);
            adj.get(v).add(e2);
        }
        
        return adj;
    }
    public int[] dijkstra(int V, int[][] edges, int src) {
        ArrayList<ArrayList<ArrayList<Integer>>> adj = constructAdj(edges, V);
        
        // PriorityQueue(Min heap) to store vertices to be processed
        // Each element is a pair: [distance, node]
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a, b) -> Integer.compare(a[0], b[0]));
        
        // Create a distance array and initialize all 
        // distances as infinite(Very Large Value)
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        
        // from src to src distance is always 0.
        dist[src] = 0;
        pq.offer(new int[]{0, src});
        
        while (!pq.isEmpty()) {
            int[] curr = pq.poll();
            int currDist = curr[0];
            int currNode = curr[1];

            // Visit all neighbors
            for (ArrayList<Integer> neighbor : adj.get(currNode)) {
                int adjNode = neighbor.get(0);
                int edgeWeight = neighbor.get(1);

                if (currDist + edgeWeight < dist[adjNode]) {
                    dist[adjNode] = currDist + edgeWeight;
                    pq.offer(new int[]{dist[adjNode], adjNode});
                }
            }
        }
        
        return dist;
    }
}
```
### Complexity Analysis

**Time Complexity** 
* Building the Adjacency List: Constructing the adjacency list takes O(E) time, where E is the number of edges.
* Priority Queue Operations:
    * Each vertex is added to the priority queue at most once, and each insertion or deletion operation takes O(log V), where V is the number of vertices.
    * For each edge, the algorithm performs a relaxation operation, which involves updating the priority queue. This takes O(E log V) in total.
* Overall Time Complexity: O((V + E) log V).

**Space Complexity**
* Adjacency List: The adjacency list requires O(V + E) space to store the graph.
* Distance Array: The dist array requires O(V) space to store the shortest distances.
* Priority Queue: The priority queue can hold up to O(V) elements at a time, requiring O(V) space.
* Overall Space Complexity: O(V + E).
