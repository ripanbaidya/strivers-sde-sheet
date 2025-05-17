# [K Max Sum Combinations](https://www.naukri.com/code360/problems/k-max-sum-combinations_975322?leftPanelTabValue=PROBLEM)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Heap         | Google, Amazon      | [Video]()   |
|            |              | Microsoft, Facebook | [Article]() |

## Description
You are given two arrays/lists ‘A’ and ‘B’ of size ‘N’ each. You are also given an integer ‘K’. You must return ‘K’ distinct maximum and valid sum combinations from all the possible sum combinations of the arrays/lists ‘A’ and ‘B’.

Sum combination adds one element from array ‘A’ and another from array ‘B’.

```
For example :
A : [1, 3] 
B : [4, 2] 
K: 2

The possible sum combinations can be 5(3 + 2), 7(3 + 4), 3(1 + 2), 5(1 + 4). 

The 2 maximum sum combinations are 7 and 5.
```
```
Sample Input 1 :
3 2
1 3 5
6 4 2
Sample Output 1 :
11 9
Explanation of Sample Output 1 :
For the given arrays/lists, all the possible sum combinations are: 
7(1 + 6), 5(1 + 4), 3(1 + 2), 9(3 + 6), 7(3 + 4), 5(3 + 2), 11(6 + 5), 9(5 + 4), 7(5 + 2).

The two maximum sum combinations from the above combinations are 11 and 9. 

Sample Input 2 :
2 1
1 1
1 1
Sample Output 2 :
2
Explanation of sample input 2 :
For the given arrays/lists, two possible sum combinations are 2(1 + 1), and 2(1 + 1).
Constraints :
1 <= N <= 100
1 <= K <= N
-10^5 <= A[i], B[i] <= 10^5

'A[i]' and 'B[i]' denote the ith element in the given arrays/lists. 

Time limit: 1 sec
```

## [Naive Approach] 

### Code
```java
public class Solution {
    public static int[] kMaxSumCombination(int []a, int []b, int n, int k){
        int[] ans = new int[k];
        PriorityQueue<Integer> maxheap = new PriorityQueue<>(Collections.reverseOrder());

        for(int i = 0; i < n; i ++){
            for(int j = 0; j < n; j ++){
                int sum = a[i] + b[j];
                maxheap.offer(sum);
            }
        }

        // k max sum
        int c = 0; // ans track

        for(int i = 0; i < k; i ++){
            ans[c ++] = maxheap.poll();
        } 

        return ans;
    }
}
```

### Complexity Analysis

- **Time Complexity** : `O(n)`
    1. **Generating all Sum**  `O(n ^ 2)` and putting all sum inside max heap. `O(n^2 log n)`
    2. **Remove k element** to get the answer `O(k log n)`.
    3. **Total Time Complexity**: `O(n² log n + k log n)`

- **Space Complexity** : 
    1. **Storing result** : `O(k)` 
    2. **store all sum** : `O(n ^ 2)` 
    3. **Total Space Complexity** : `O(n² + k)`


## [Better Approach] Space Optimization

### Code
```java
public static int[] kMaxSumCombination(int[] a, int[] b, int n, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int sum = a[i] + b[j];
            if (minHeap.size() < k) {
                minHeap.offer(sum);
            } else if (sum > minHeap.peek()) {
                minHeap.poll();
                minHeap.offer(sum);
            }
        }
    }

    int[] result = new int[k];
    for (int i = k - 1; i >= 0; i--) {
        result[i] = minHeap.poll();
    }

    return result;
}
```
### Complexity Analysis
- **Time Complexity** : `O(n^2 log k)`
- **Space Complexity** : `O(k)`


## [Optimal Approach]

### Intuition
The initial approach is inefficient for large `n` because it checks all `n^2` pairs. However, we don't need all pairs; we only need the top `k` sums. 

Key observations:
- The largest sum is `a[max] + b[max]`.
- The next largest sums could be `a[max] + b[second max]` or `a[second max] + b[max]`, and so on.
- We can use a max-heap to explore sums in descending order without generating all pairs upfront.

### Algorithm

1. **Sort Both Arrays:** Sort arrays `a` and `b` in descending order. This helps in efficiently accessing the largest elements.
2. **Max-Heap for Candidates:** Use a max-heap (priority queue) to store potential candidates, starting with the largest possible sum `(a[0] + b[0])` along with the indices `(0, 0)`.
3. **Avoid Duplicate Pairs:** Use a hash set to keep track of visited index pairs to avoid processing the same pair multiple times.
4. **Extract Top `k` Sums:** Extract the top `k` sums from the heap, and for each extracted sum `(a[i] + b[j])`, consider the next possible candidates `(a[i+1] + b[j])` and `(a[i] + b[j+1])` if they haven't been visited.

### Code
```java
class Solution{
   public static int[] kMaxSumCombination(int[] a, int[] b, int n, int k) {
       // Sort both arrays in descending order
       Arrays.sort(a);
       Arrays.sort(b);
       reverseArray(a);
       reverseArray(b);

       // Max-heap based on the sum
       PriorityQueue<int[]> maxHeap = new PriorityQueue<>((x, y) -> (y[0] - x[0]));
       // Visited set to avoid duplicate pairs
       Set<String> visited = new HashSet<>();

       // Initialize with the largest sum (a[0] + b[0])
       maxHeap.offer(new int[]{a[0] + b[0], 0, 0});
       visited.add(0 + "," + 0);

       int[] result = new int[k];
       int index = 0;

       while (index < k && !maxHeap.isEmpty()) {
           int[] current = maxHeap.poll();
           int sum = current[0];
           int i = current[1];
           int j = current[2];

           result[index++] = sum;

           // Next candidate: (i+1, j)
           if (i + 1 < n && !visited.contains((i + 1) + "," + j)) {
               maxHeap.offer(new int[]{a[i + 1] + b[j], i + 1, j});
               visited.add((i + 1) + "," + j);
           }

           // Next candidate: (i, j+1)
           if (j + 1 < n && !visited.contains(i + "," + (j + 1))) {
               maxHeap.offer(new int[]{a[i] + b[j + 1], i, j + 1});
               visited.add(i + "," + (j + 1));
           }
       }

       return result;
   }

   private static void reverseArray(int[] arr) {
       int left = 0, right = arr.length - 1;
       while (left < right) {
           int temp = arr[left];
           arr[left] = arr[right];
           arr[right] = temp;
           left++;
           right--;
       }
   }
}
```

### Complexity Analysis

- **Time Complexity:** 
  - Sorting both arrays: `O(n log n)`.
  - Extracting `k` elements from the heap: Each extraction and insertion takes `O(log k)`, leading to `O(k log k)`.
  - Overall: `O(n log n + k log k)` (dominated by the sorting step for large `n`).
- **Space Complexity:** 
  - `O(k)` for the heap and visited set.
