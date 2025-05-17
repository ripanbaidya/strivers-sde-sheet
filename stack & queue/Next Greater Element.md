# [Next Greater Element](https://www.naukri.com/code360/problems/next-greater-element_670312)

| Difficulty | Topic           | Companies           | Resources   |
| ---------- | --------------- | ------------------- | ----------- |
| **Medium** | Monotonic Stack | Amazon           | [Video](https://youtu.be/e7XQLtOQM3I?si=qnfLuTpTqCMuVJgO)   |
|            | Arrays          | Microsoft, Facebook | [Article](https://www.geeksforgeeks.org/next-greater-element/) |

## Description
You are given an array 'a' of size 'n'.
Print the Next Greater Element(NGE) for every element.

The Next Greater Element for an element 'x' is the first element on the right side of 'x' in the array, which is greater than 'x'.

If no greater elements exist to the right of 'x', consider the next greater element as -1.

```
For example:
Input: 'a' = [7, 12, 1, 20]
Output: NGE = [12, 20, 20, -1]

Explanation: For the given array,
- The next greater element for 7 is 12.
- The next greater element for 12 is 20. 
- The next greater element for 1 is 20. 
- There is no greater element for 20 on the right side. So we consider NGE as -1.
Detailed explanation ( Input/output format, Notes, Images )
```

**Constraints** :
- 1 <= 'n' <= 10^5
- 1 <= 'a[i]' <= 10^9


### Intuition

We are given an array, and for each element, we need to find the **Next Greater Element (NGE)** — the first element to the **right** that is **greater** than the current element.

To solve this efficiently, we use a **stack** to keep track of potential "next greater" candidates. By iterating from **right to left**, we always maintain a stack such that:
- The top of the stack is the **next greater element** for the current element, if it exists.
- Elements smaller than or equal to the current element are not useful for future comparisons and are removed from the stack.


### Algorithm

1. Initialize a result array `nge` of size `n`, where `nge[i]` will hold the next greater element for `arr[i]`.
2. Create an empty stack `st` to keep track of elements in decreasing order.
3. Traverse the array from right to left:
   - Let `current = arr[i]`
   - Pop elements from the stack while `st.peek() <= current` — because they cannot be the next greater for `current` or any element to its left.
   - If the stack is empty after popping, there's no greater element → set `nge[i] = -1`.
   - Else, the top of the stack is the next greater → set `nge[i] = st.peek()`.
   - Push `current` onto the stack (it may be the NGE for upcoming elements).
4. Return the `nge` array.



### Code
```java
public class Solution {
    public int[] nextGreaterElement(int[] arr, int n) {
        int[] nge = new int[n];
        Stack<Integer> st = new Stack<>();

        for(int i = n - 1; i >= 0; i--) {
            int current = arr[i];

            // Remove elements that are not greater than the current element
            while(!st.isEmpty() && st.peek() <= current) {
                st.pop();
            }

            // If stack is empty, no greater element exists
            nge[i] = st.isEmpty() ? -1 : st.peek();

            // Push current element for next iteration
            st.push(current);
        }
        return nge;
    }
}
```

### Complexity Analysis

- **Time Complexity**: `O(n)`  
  Each element is pushed and popped from the stack at most once.
- **Space Complexity**: `O(n)`  
  We use extra space for the result array and the stack, both of which can grow up to `n` elements in the worst case.
