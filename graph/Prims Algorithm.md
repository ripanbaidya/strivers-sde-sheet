# [Prim’s Algorithm for Minimum Spanning Tree (MST)](https://www.geeksforgeeks.org/problems/minimum-spanning-tree/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Greedy, Graph| Flipkart            | [Video](https://youtu.be/mJcZjjKzeqk)   |
|            | Algorithm    | Microsoft           | [Article](https://takeuforward.org/data-structure/prims-algorithm-minimum-spanning-tree-c-and-java-g-45/) |


## Description
Given a weighted, undirected, and connected graph with V vertices and E edges, your task is to find the sum of the weights of the edges in the Minimum Spanning Tree (MST) of the graph. The graph is represented by an adjacency list, where each element adj[i] is a vector containing vector of integers. Each vector represents an edge, with the first integer denoting the endpoint of the edge and the second integer denoting the weight of the edge.

<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700343/Web/Other/blobid1_1744376821.jpg" height=200 width=220> ==>
<img src="https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700343/Web/Other/blobid2_1744376854.jpg" height=200 width=220>

```
Input:
3 3
0 1 5
1 2 3
0 2 1
 
Output: 4

Explanation:
The Spanning Tree resulting in a weight
of 4 is shown above.
```

**Constraints:**
* 2 ≤ V ≤ 1000
* V-1 ≤ E ≤ (V*(V-1))/2
* 1 ≤ w ≤ 1000
* The graph is connected and doesn't contain self-loops & multiple edges.

## Optimized Implementation using Adjacency List Representation (of Graph) and Priority Queue(Min Heap)

### Articles
* <a href="https://takeuforward.org/data-structure/prims-algorithm-minimum-spanning-tree-c-and-java-g-45/" target="_blank">TakeUforward</a>

### Code
```java
class Pair {
    int distance;
    int node;

    public Pair(int distance, int node) {
        this.distance = distance;
        this.node = node;
    }
}

class Solution {
    static int spanningTree(int V, int E, List<List<int[]>> adj) {
        // PriorityQueue to store edges (node, distance) with minimum distance at the top.
        PriorityQueue<Pair> pq = new PriorityQueue<>((a, b) -> a.distance - b.distance);

        int[] vis = new int[V];
        int mstWeight = 0;

        // Start with vertex 0 and distance 0.
        pq.offer(new Pair(0, 0));

        // Iterate until all reachable vertices are included in the MST.
        while (!pq.isEmpty()) {
            // Get the edge with the smallest distance.
            Pair current = pq.poll();
            int currentDistance = current.distance;
            int currentNode = current.node;

            // If the node is already visited, skip it.  This prevents cycles.
            if (vis[currentNode] == 1) {
                continue;
            }

            // Mark the node as visited.
            // and Add the distance to the MST weight.
            vis[currentNode] = 1;
            mstWeight += currentDistance;

            // Explore the neighbors of the current node.
            for (int[] neighbor : adj.get(currentNode)) {
                int neighborNode = neighbor[0];
                int weightToNeighbor = neighbor[1];

                // If the neighbor is not visited, add it to the priority queue.
                if (vis[neighborNode] == 0) {
                    pq.offer(new Pair(weightToNeighbor, neighborNode));
                }
            }
        }
        return mstWeight;
    }
}
```

### Complexity Analysis

- **Time Complexity:** `O(E*logE) + O(E*logE)~ O(E*logE)`, where E = no. of given edges.
The maximum size of the priority queue can be E so after at most E iterations the priority queue will be empty and the loop will end. Inside the loop, there is a pop operation that will take logE time. This will result in the first O(E*logE) time complexity. Now, inside that loop, for every node, we need to traverse all its adjacent nodes where the number of nodes can be at most E. If we find any node unvisited, we will perform a push operation and for that, we need a logE time complexity. So this will result in the second O(E*logE). 

- **Space Complexity:** `O(E) + O(V)`, where E = no. of edges and V = no. of vertices. O(E) occurs due to the size of the priority queue and O(V) due to the visited array. If we wish to get the mst, we need an extra O(V-1) space to store the edges of the most.