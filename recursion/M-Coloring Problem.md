# [M-Coloring Problem](https://www.geeksforgeeks.org/problems/m-coloring-problem-1587115620/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Backtracking | Google, Amazon      | [Video](https://youtu.be/wuVwUK25Rfc?si=Mhf9Q8G9TVlU50P6)   |
|            | Graph        | Microsoft, Facebook | [Article](https://www.geeksforgeeks.org/m-coloring-problem/) |

## Description
You are given an undirected graph consisting of V vertices and E edges represented by a list edges[][], along with an integer m. Your task is to determine whether it is possible to color the graph using at most m different colors such that no two adjacent vertices share the same color. Return true if the graph can be colored with at most m colors, otherwise return false.

**Note:** The graph is indexed with 0-based indexing.

**Examples:**
```
Example 1:
Input: V = 4, edges[][] = [[0, 1], [1, 2], [2, 3], [3, 0], [0, 2]], m = 3
Output: true
Explanation: It is possible to color the given graph using 3 colors, for example, one of the possible ways vertices can be colored as follows:
Vertex 0: Color 3
Vertex 1: Color 2
Vertex 2: Color 1
Vertex 3: Color 2

Example 2:
Input: V = 3, edges[][] = [[0, 1], [1, 2], [0, 2]], m = 2
Output: false
Explanation: It is not possible to color the given graph using only 2 colors because vertices 0, 1, and 2 form a triangle.
```

**Constraints:**
- 1 ≤ V ≤ 10
- 1 ≤ E = edges.size() ≤ (V*(V- 1))/2
- 0 ≤ edges[i][j] ≤ V - 1
- 1 ≤ m ≤ V

## [Efficient Approach] Using Backtracking – O(V * m^V) Time and O(V+E) Space
Assign colors one by one to different vertices, starting from vertex 0. Before assigning a color, check for safety by considering already assigned colors to the adjacent vertices i.e check if the adjacent vertices have the same color or not. If there is any color assignment that does not violate the conditions, mark the color assignment as part of the solution. If no assignment of color is possible then backtrack and return false

### Code
```java
class Solution {
    // construct adjacency list
    public List<List<Integer>> constructAdj(int V, int[][] edges){
        List<List<Integer>> adj = new ArrayList<>();
        
        // create V empty lists
        for(int i = 0; i < V; i ++){
            adj.add(new ArrayList<>());
        }
        
        // add edges to adjacency list
        for(int[] edge : edges){
            int u = edge[0];
            int v = edge[1];
            
            adj.get(u).add(v);
            adj.get(v).add(u); // undirect 
        }
        return adj;
    }
    public boolean isPossible(int node, int curColor, List<List<Integer>> adj, int[] color){
        // check if the current color is used by any of
        // the adjacent nodes
        for(int neighbor : adj.get(node)){
            if(color[neighbor] != -1 && color[neighbor] == curColor){
                return false;
            }
        }
        return true;
    }
    // check graph is colorable or not
    public boolean colorGraph(int node, int v, List<List<Integer>> adj, int m, int[] color){
        // base case
        if(node == v){
            // all node has been colored
            return true;
        }
        
        // try all possible color
        for(int i = 0; i < m; i ++){
            if(isPossible(node, i, adj, color)){
                color[node] = i;
                if(colorGraph(node+1, v, adj, m, color) == true)
                    return true;
                
                // backtrack to previous node
                // and make sure remove the assigned color
                color[node] = -1;
            }
        }
        
        return false;    
    }
    boolean graphColoring(int v, int[][] edges, int m) {
        List<List<Integer>> adj = constructAdj(v, edges);
        
        // track the node that has colored
        // initially fill then by -1
        // no node has been coloured right now
        int[] color = new int[v];
        Arrays.fill(color, -1);
        
        // {startingNode, noOfVertices, adjList, mColor, colorArray}
        return colorGraph(0, v, adj, m, color);
    }
}
```

### Complexity Analysis

- **Time Complexity**: O(V * m^v). There is a total of O(m^v) combinations of colors. For each attempted coloring of a vertex you call isPossible(), can have up to V–1 neighbors, so isPossible() is O(V)

- **Auxiliary Space**: O(V + E). The recursive Stack of the graph coloring function will require O(V) space, Adjacency list and color array will required O(V+E).