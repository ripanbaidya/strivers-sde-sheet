# [Subset Sums](https://www.geeksforgeeks.org/problems/subset-sums2234/1)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Recursion    |  Amazon             | [Video](https://youtu.be/rYkfBRtMJr8?si=TURlXngYNxF_T1QE)   |
|            |              | Microsoft           | [Article](https://www.geeksforgeeks.org/subset-sum-problem-dp-25/) |

## Description

Given an array arr[] of non-negative integers and a value sum, the task is to check if there is a subset of the given array whose sum is equal to the given sum. 

**Examples:** 
```
Input: arr[] = [3, 34, 4, 12, 5, 2], sum = 9
Output: True
Explanation: There is a subset (4, 5) with sum 9.


Input: arr[] = [3, 34, 4, 12, 5, 2], sum = 30
Output: False
Explanation: There is no subset that add up to 30.
```

## Optimal Approach

### Intuition

The problem is to find the **sum of all subsets** of a given array. Each element has two choices — either to be **included** in a subset or **excluded**. This leads to `2^n` possible subsets (where `n` is the size of the array).

We use **backtracking/recursion** to explore all these possibilities and accumulate the sum of each subset into a list. Finally, we **sort** this list to return the subset sums in increasing order.


### Algorithm

1. **Define a recursive function** `helper(idx, arr, sum, ans)`:
   - `idx`: current index in the array.
   - `sum`: current sum of the subset being formed.
   - `ans`: list storing all subset sums.

2. **Base case**:  
   If `idx == arr.length`, we’ve considered all elements, so add `sum` to the result list `ans`.

3. **Recursive case**:  
   - **Include** `arr[idx]` in the current subset and call `helper(idx+1, arr, sum + arr[idx], ans)`.
   - **Exclude** `arr[idx]` from the current subset and call `helper(idx+1, arr, sum, ans)`.

4. In the main function `subsetSums(int[] arr)`:
   - Create an empty `ArrayList<Integer>` to store answers.
   - Call the helper function with initial index `0` and sum `0`.
   - Sort the result list before returning.


### Code
```java
class Solution {
    /**
     * Recursive helper function to generate all subset sums.
     */
    private void generateAllSubsetSums(int currentIndex, int[] arr, int currentSum, List<Integer> subsetSums) {
        if (currentIndex == arr.length) {
            subsetSums.add(currentSum);
            return;
        }

        // Pick up the current element
        int newSum = currentSum + arr[currentIndex];
        generateAllSubsetSums(currentIndex + 1, arr, newSum, subsetSums);

        // Don't pick up the current element
        generateAllSubsetSums(currentIndex + 1, arr, currentSum, subsetSums);
    }

    /**
     * Function to generate all subset sums of a given array.
     */
    public List<Integer> subsetSums(int[] arr) {
        List<Integer> subsetSums = new ArrayList<>();
        generateAllSubsetSums(0, arr, 0, subsetSums);

        Collections.sort(subsetSums); // sort the answer

        return subsetSums;
    }
}
```

### Complexity Analysis

- **Time Complexity**
  - The number of subsets of an array of length `n` is `2^n`.
  - For each subset, we do constant-time operations to calculate and store its sum.
  - At the end, we sort the `2^n` subset sums.
  - Total Time Complexity:  `O(2^n log 2^n)`, O(2^n) for generating subsets + O(2^n log 2^n) for sorting  

- **Space Complexity** 
  - We store `2^n` subset sums in the `ans` list → O(2^n).
  - Recursion stack space is O(n) (due to depth of recursion), but since `O(2^n)` dominates, we consider:
  - Total Space Complexity:  `O(n ^ 2)`

