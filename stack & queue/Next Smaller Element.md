# [Next Smaller Element](https://www.naukri.com/code360/problems/next-smaller-element_1112581?leftPanelTabValue=PROBLEM)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     |MonotonicStack|                     | [Video]()   |
|            |              | Microsoft, Facebook | [Article](https://www.geeksforgeeks.org/next-smaller-element/) |

## Description
You are given an array 'ARR' of integers of length N. Your task is to find the next smaller element for each of the array elements.

Next Smaller Element for an array element is the first element to the right of that element which has a value strictly smaller than that element.

If for any array element the next smaller element does not exist, you should print -1 for that array element.

For Example:

If the given array is [ 2, 3, 1], we need to return [1, 1, -1]. Because for  2, 1 is the Next Smaller element. For 3, 1 is the Next Smaller element and for 1, there is no next smaller element hence the answer for this element is -1.

``` 
Example: 

Input: 2 1 4 3
Output: 1 -1 3 -1

For this test case : 
1) For ARR [1] = 2 ,  the next smaller element is 1. 
2) For ARR [2] = 1 ,  the next smaller element is -1 as no element in the array has value smaller than 1.
3) For ARR [3] = 4 ,  the next smaller element is 3.
4) For ARR [4] = 3 ,  the next smaller element is -1 as no element exists in the right of it.
Hence, we will return the array [ 1, -1, 3, -1] in this case.
```
**Constraints:**
- 1 <= T <= 10
- 1 <= N <= 10 ^ 5
- 0 <= ARR [i] <= 10 ^ 5


## [Expected Approach] using Stack – O(n) Time O(n) Space
The idea is to find the next smaller element for each element in the list using a monotonic stack. We traverse the list while maintaining a stack that keeps indices of elements in decreasing order. If the current element is smaller than the element at the stack’s top, it becomes the next smaller element for that index. We then update the result and pop the stack until this condition no longer holds. If no smaller element exists, we keep -1. 

### Code
```java
public class NextSmallerElement {
    public static ArrayList<Integer> findNextSmallerElements(ArrayList<Integer> input, int n) {
        ArrayList<Integer> result = new ArrayList<>(Collections.nCopies(n, -1));
        Stack<Integer> elementStack = new Stack<>();

        for (int i = n - 1; i >= 0; i--) {
            int currentElement = input.get(i);

            while (!elementStack.isEmpty() && currentElement <= elementStack.peek()) {
                elementStack.pop();
            }

            int nextSmallerElement = elementStack.isEmpty() ? -1 : elementStack.peek();
            result.set(i, nextSmallerElement);

            elementStack.push(currentElement);
        }

        return result;
    }
}
```

### Complexity Analysis
- `Time Complexity` : `O(n)`
- `Space Complexiy` : `O(n)`