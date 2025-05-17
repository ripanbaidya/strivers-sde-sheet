# The Celebrity Problem

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Stack        | Google, Amazon      | [Video](https://youtu.be/OZPmEA_8FM8?si=hRLSK7MHW8H2Dz7D)   |
|            |              | Microsoft, Facebook | [Article](https://www.geeksforgeeks.org/the-celebrity-problem/) |

## Description
A celebrity is a person who is known to all but does not know anyone at a party. A party is being organized by some people. A square matrix mat[][] (n*n) is used to represent people at the party such that if an element of row i and column j is set to 1 it means ith person knows jth person. You need to return the index of the celebrity in the party, if the celebrity does not exist, return -1.

**Note:** Follow 0-based indexing.

**Examples:**

```
Example 1:
Input: mat[][] = [[1, 1, 0], [0, 1, 0], [0, 1, 1]]
Output: 1
Explanation: 0th and 2nd person both know 1st person. Therefore, 1 is the celebrity person.

Example 2:
Input: mat[][] = [[1, 1], [1, 1]]
Output: -1
Explanation: Since both the people at the party know each other. Hence none of them is a celebrity person.

Example 3:
Input: mat[][] = [[1]]
Output: 0
```

**Constraints**

- 1 <= mat.size()<= 1000
- 0 <= mat[i][j]<= 1
- mat[i][i] == 1


## [Naive Approach] – Using Adjacency List – O(n^2) Time and O(n) Space
Analyze the problem using Graphs. Initialize indegree and outdegree of every vertex as 0. 

* If A knows B, draw a directed edge from A to B, increase indegree of B and outdegree of A by 1. 
* Construct all possible edges of the graph for every possible pair [i, j]. 
* There are nC2 pairs. If a celebrity is present in the party, there will be one node in the graph which has the outdegree as zero and indegree equals to N-1. 

### Algorithm
- Create two arrays indegree and outdegree, to store the indegree and outdegree
- Run a nested loop, the outer loop from 0 to n and inner loop from 0 to n.
- For every pair i, j check if i knows j then increase the outdegree of i and indegree of j.
- For every pair i, j check if j knows i then increase the outdegree of j and indegree of i.
- Run a loop from 0 to n and find the id where the indegree is n-1 and outdegree is 0.

### Code
```java
class Solution {

    // Function to find the celebrity
    static int celebrity(int[][] mat) {
        int n = mat.length;

        // indegree and outdegree array
        int[] indegree = new int[n];
        int[] outdegree = new int[n];

        // query for all edges
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int x = mat[i][j];

                // set the degrees
                outdegree[i] += x;
                indegree[j] += x;
            }
        }

        // find a person with indegree n-1
        // and out degree 0
        for (int i = 0; i < n; i++)
            if (indegree[i] == n - 1 && outdegree[i] == 0)
                return i;

        return -1;
    }

    public static void main(String[] args) {
        int[][] mat = { { 0, 1, 0 },
                        { 0, 0, 0 },
                        { 0, 1, 0 } };
        System.out.println(celebrity(mat));
    }
}
```

### Complexity Analysis
- **Time Complexity** : `O(n ^ 2s)`
- **Space Complexiy** : `O(n)`



## [Efficient Approach] – Using Stack – O(n) Time and O(n) Space

### Intution
Some observations are based on Stack Based elimination technique :

* If A knows B, then A can’t be a celebrity. Discard A, and B may be celebrity.
* If A doesn’t know B, then B can’t be a celebrity. Discard B, and A may be celebrity.
* Repeat above two steps till there is only one person.
* Ensure the remained person is a celebrity. 

### Algorithm
1. Create a stack and push all the ids in the stack.
2. Run a loop while there are more than 1 element in the stack.
3. Pop the top two elements from the stack (represent them as A and B)
4. If A knows B, then A can’t be a celebrity and push B in the stack. Else if A doesn’t know B, then B can’t be a celebrity push A in the stack.
5. Assign the remaining element in the stack as the celebrity.
6. Run a loop from 0 to n-1 and find the count of persons who knows the celebrity and the number of people whom the celebrity knows.
    * If the count of persons who knows the celebrity is n-1 and the count of people whom the celebrity knows is 0 then return the id of the    celebrity else return -1.
  
### Code
```java
class Solution{
    // Function to find the celebrity
    static int celebrity(int[][] mat) {
        int n = mat.length;
        Stack<Integer> st = new Stack<>();

        // Push everybody in stack
        for (int i = 0; i < n; i++)
            st.push(i);

        // Find a potential celebrity
        while (st.size() > 1) {
            int a = st.pop();
            int b = st.pop();

            // if a knows b
            if (mat[a][b] != 0) {
                st.push(b);
            } else {
                st.push(a);
            }
        }

        // Potential candidate of celebrity
        int c = st.pop();

        // Check if c is actually a celebrity or not
        for (int i = 0; i < n; i++) {
            if(i == c) continue;

            // If any person doesn't know 'c' or 'c' doesn't know any person, return -1
            if (mat[c][i] != 0 || mat[i][c] == 0){
                return -1;
            }  
        }
        return c;
    }
}
```
### Complexity Analysis
- **Time Complexity** : `O(n)`
- **Space Complexiy** : `O(n)`
