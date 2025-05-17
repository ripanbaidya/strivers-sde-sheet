# [Bellman-Ford Algorithm](https://www.geeksforgeeks.org/problems/distance-from-the-source-bellman-ford-algorithm/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Graph        | Flipkart            | [Video](https://youtu.be/0vVofAhAYjc)   |
|            | Algorithm    | Microsoft           | [Article](https://www.geeksforgeeks.org/bellman-ford-algorithm-dp-23/) |

## Description
Given an weighted graph with V vertices numbered from 0 to V-1 and E edges, represented by a 2d array edges[][], where edges[i] = [u, v, w] represents a direct edge from node u to v having w edge weight. You are also given a source vertex src.

Your task is to compute the shortest distances from the source to all other vertices. If a vertex is unreachable from the source, its distance should be marked as 108. Additionally, if the graph contains a negative weight cycle, return [-1] to indicate that shortest paths cannot be reliably computed.

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/893096/Web/Other/blobid0_1744455175.jpg" height=300 width=300>

```
Input: V = 5, edges[][] = [[1, 3, 2], [4, 3, -1], [2, 4, 1], [1, 2, 1], [0, 1, 5]], src = 0
Output: [0, 5, 6, 6, 7]
Explanation: Shortest Paths:
For 0 to 1 minimum distance will be 5. By following path 0 → 1
For 0 to 2 minimum distance will be 6. By following path 0 → 1  → 2
For 0 to 3 minimum distance will be 6. By following path 0 → 1  → 2 → 4 → 3 
For 0 to 4 minimum distance will be 7. By following path 0 → 1  → 2 → 4
```

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/893096/Web/Other/blobid1_1744455218.jpg" height=300 width=300>

```
Input: V = 4, edges[][] = [[0, 1, 4], [1, 2, -6], [2, 3, 5], [3, 1, -2]], src = 0
Output: [-1]
Explanation: The graph contains a negative weight cycle formed by the path 1 → 2 → 3 → 1, where the total weight of the cycle is negative.
```

**Constraints**:
* 1 ≤ V ≤ 100
* 1 ≤ E = edges.size() ≤ V*(V-1)
* -1000 ≤ w ≤ 1000
* 0 ≤ src < V

### Articles
* <a href="https://www.geeksforgeeks.org/bellman-ford-algorithm-dp-23/" target="_blank">GeeksforGeeks</a>
* <a href="https://takeuforward.org/data-structure/bellman-ford-algorithm-g-41/" target="_blank">TakeUforward</a>

### Code
```java
class Solution {
    public int[] bellmanFord(int V, int[][] edges, int src) {
        int[] dist = new int[V];
        Arrays.fill(dist, (int)1e8);
        dist[src] = 0; 
       
        for(int i = 0; i < V-1; i ++){
            for(int[] edge : edges){
                int u = edge[0];
                int v = edge[1];
                int wt = edge[2];
                
                
                if(dist[u] != (int)1e8 && dist[u]+wt < dist[v]){
                    dist[v] = dist[u] + wt;
                }
            }
        }
       
        // N-th relaxation to check the negative values
        for(int[] edge : edges){
            int u = edge[0];
            int v = edge[1];
            int wt = edge[2];
           
            if(dist[u] != (int)1e8 && dist[u]+wt < dist[v]){
                return new int[]{-1};
            }
        }
        
        return dist;
    }
}
```

### Complexity Analysis
**Time Complexity**

* Outer Loop: The algorithm iterates V-1 times, where V is the number of vertices.
* Inner Loop: For each iteration, it processes all the edges in the graph. If there are E edges, this takes O(E) time.
* Negative Cycle Check: After the V-1 iterations, the algorithm performs one more pass over all edges to check for negative weight cycles, which takes O(E) time.
* Thus, the total time complexity is `O(V * E)`.

**Space Complexity**
* The algorithm uses an array dist of size V to store the shortest distances from the source to all vertices.
* No additional space is used apart from the input graph representation.
* Thus, the space complexity is `O(V)`.

