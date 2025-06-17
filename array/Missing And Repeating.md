# [Missing And Repeating](https://www.geeksforgeeks.org/problems/find-missing-and-repeating2512/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=practice_card)

| Difficulty | Topics           | Companies | Video                                                      |
| ---------- | ---------------- | --------- | -----------------------------------------------------------|
| **Medium** | Array, Matrix    |           | [Video](https://youtu.be/2D0D8HE6uak?si=SdkaQT9bIsMQTy-b)  |
|            | Hash Table       |           | [Article](https://www.geeksforgeeks.org/find-a-repeating-and-a-missing-number/) |                                                    

## Question Variations

1.  [Missing And Repeating](https://www.geeksforgeeks.org/problems/find-missing-and-repeating2512/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=practice_card)

2. [2965. Find Missing and Repeated Values](https://leetcode.com/problems/find-missing-and-repeated-values/description/)

## Description

Given an unsorted array `arr` of positive integers, one number `a` from the set `[1, 2, ..., n]` is missing, and one number `b` occurs twice in the array. Find the numbers `a` (missing) and `b` (repeating).

**Note**: The test cases are generated such that there always exists one missing and one repeating number within the range `[1, n]`.

**Examples**
```
Input: arr[] = [2, 2]
Output: [2, 1]
Explanation: Repeating number is 2 and smallest positive missing number is 1.

Input: arr[] = [1, 3, 3] 
Output: [3, 2]
Explanation: Repeating number is 3 and smallest positive missing number is 2.

Input: arr[] = [4, 3, 6, 2, 1, 1]
Output: [1, 5]
Explanation: Repeating number is 1 and the missing number is 5.
```

**Constraints**
- `2 ≤ arr.size() ≤ 10^6`
- `1 ≤ arr[i] ≤ arr.size()`


## Using Visited Array – O(n) time and O(n) space

### Intuition
We can use a hash array to count the occurrences of each number in the array. The number with a count of `2` is the repeating number, and the number with a count of `0` is the missing number.

### Approach
1. Create a hash array of size `n + 1` to count occurrences of each number.
2. Traverse the array and update the count of each number in the hash array.
3. Traverse the hash array to find the repeating and missing numbers.

### Code
```java
class Solution {
    ArrayList<Integer> findTwoElement(int arr[]) {
        int n = arr.length;

        // Create a Hash array with size (n+1)
        int[] hash = new int[n + 1]; 
        int missing = -1, repeating = -1;

        // {repeat, missing}
        ArrayList<Integer> res = new ArrayList<>(); 

        // Count occurrences of each number
        for (int i = 0; i < n; i++) {
            // Increment the count
            hash[arr[i]]++; 
        }

        // start with index 1, 0 is not in the range
        for (int i = 1; i < hash.length; i++) {
            if (hash[i] == 2) {
                // Repeating number
                repeating = i; 
            }
            if (hash[i] == 0) {
                // Missing number
                missing = i;
            }

            if (missing != -1 && repeating != -1) {
                break;
            }
        }

        // Add repeating and missing values to the resultant list
        res.add(repeating);
        res.add(missing);

        return res;
    }
}
```

### Complexity Analysis
- **Time Complexity**: `O(n)`  
- **Space Complexity**: `O(n)`  


## Making Two Math Equations – O(n) time and O(1) space

### Intuition
We can use mathematical equations to find the repeating and missing numbers:
1. Let `x` be the repeating number and `y` be the missing number.
2. Calculate the sum of the array (`S`) and the sum of squares of the array (`S2`).
3. Calculate the sum of N (`SN`) and the sum of squares of N (`S2N`).
4. Use the formulas:
   - `x - y = s - sn` (where `sn` is the sum of the first `n` natural numbers).
   - `x^2 - y^2 = s2 - s2n` (where `s2n` is the sum of squares of the first `n` natural numbers).
5. Solve the equations to find `x` and `y`.

### Approach
1. Calculate the sum of the array (`s`) and the sum of squares of the array (`s2`).
2. Calculate the sum of the first `n` natural numbers (`sn`) and the sum of squares of the first `n` natural numbers (`s2n`).
3. Use the equations to find `x` and `y`.

### Code
```java
class Solution {
    ArrayList<Integer> findTwoElement(int arr[]) {
        // x = repeating, y = missing
        long n = arr.length;

        // Sum of first n natural numbers
        long sn = n * (n + 1) / 2;

        // Sum of squares of first n natural numbers
        long s2n = n * (n + 1) * (2 * n + 1) / 6;

        // Calculate current sum and sum of squares
        long s = 0, s2 = 0;
        for (int i = 0; i < n; i++) {
            s += arr[i]; // Sum of array elements
            s2 += (long) arr[i] * arr[i]; // Sum of squares of array elements
        }

        // Solve the Equations:
        // x - y = s - sn
        long firstEqn = s - sn; // x - y
        
        /**
         * (x^2 - y^2) = (s2 - s2n)
         * (x - y)(x + y) = (s2 - s2n)
         * (x + y) = (s2 - s2n)/ (x-y)
         */
        long secondEqn = (s2 - s2n) / firstEqn; // x + y

        // Solve for x and y
        long x = (firstEqn + secondEqn) / 2; // Repeating number
        long y = secondEqn - x; // Missing number

        // Prepare the result
        ArrayList<Integer> res = new ArrayList<>();
        res.add((int) x);
        res.add((int) y);

        return res;
    }
}
```

### Complexity Analysis
- **Time Complexity**: `O(n)`  
  - We traverse the array once to calculate the sum and sum of squares.
- **Space Complexity**: `O(1)`  
  - We use only a few variables to store intermediate results.

