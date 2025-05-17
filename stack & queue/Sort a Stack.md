# Sort Stack

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Stack        | Amazon   			  | [Video](https://www.geeksforgeeks.org/sort-a-stack-using-recursion/)   |
|            | Recursion    | Microsoft           | [Article](https://www.geeksforgeeks.org/sort-a-stack-using-recursion/) |


## Descroption
Given a stack, the task is to sort it using recursion.

```
Input: [3 2 1]
Output: [3 2 1]
Explanation: The given stack is sorted know 3 > 2 > 1

Input: [11 2 32 3 41]
Output: [41 32 11 3 2]
## Recursive approach
```

### Code
```java
public class Solution {
	public static void sortedInsert(Stack<Integer> s, int x) {
        if (s.isEmpty() || x > s.peek()) {
            s.push(x);
            return;
        }
        int temp = s.pop();
        sortedInsert(s, x);
        s.push(temp);
    }

    public static void sort(Stack<Integer> s) {
        if (!s.isEmpty()) {
            int x = s.pop();
            sort(s);
            sortedInsert(s, x);
        }
    }
}
```

### Complexity Analysis

- Time Complexity : `O(n)`
- Space Complexiy : `O(n)`
