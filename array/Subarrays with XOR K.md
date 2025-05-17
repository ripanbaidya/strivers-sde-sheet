#  [Subarrays with XOR ‘K’](https://www.geeksforgeeks.org/problems/count-subarray-with-given-xor/1)

| Difficulty | Topics       | Companies     | Video                                     |
| ---------- | ------------ | ------------- | ----------------------------------------- |
| **Medium** | Array        | **Google**    | [Video](https://youtu.be/lO9R5CaGRPY?si=tCOA6H2S69aUB2I4) |   
|            |              | **Uber**      | [Article](https://www.geeksforgeeks.org/count-number-subarrays-given-xor/)|

## Description

Given an array of integers arr[] and a number k, count the number of subarrays having XOR of their elements as k.

**Examples**

```
Input: arr[] = [4, 2, 2, 6, 4], k = 6
Output: 4
Explanation: The subarrays having XOR of their elements as 6 are [4, 2], [4, 2, 2, 6, 4], [2, 2, 6], and [6]. Hence, the answer is 4.

Input: arr[] = [5, 6, 7, 8, 9], k = 5
Output: 2
Explanation: The subarrays having XOR of their elements as 5 are [5] and [5, 6, 7, 8, 9]. Hence, the answer is 2.

Input: arr[] = [1, 1, 1, 1], k = 0
Output: 4
Explanation: The subarrays are [1, 1], [1, 1], [1, 1] and [1, 1, 1, 1].
```

**Constraints:**

- The problem requires an efficient solution to handle large inputs.
- The array `A` can contain both positive and negative integers.
- The value of `B` can also be positive or negative.


## [Naive Approach]: Checking all Subarray – O(n^2) Time and O(1) Space

### Intution
A Simple Solution is to use two loops to go through all possible subarrays of `arr[]` and count the number of subarrays having `XOR` of their elements as k. 

### Code
``` Java
public class Solution {
    // Function to find count of subarrays of arr 
  	// with XOR value equals to K
    static int subarrayXor(int[] arr, int k) {
        int res = 0;

        // Pick starting point i of subarrays
        for (int i = 0; i < arr.length; i++) {
            int prefXOR = 0;

            // Pick ending point j of subarray for each i
            for (int j = i; j < arr.length; j++) {
                // calculate prefXOR for subarray arr[i ... j]
                prefXOR = prefXOR ^ arr[j];

                // If prefXOR is equal to given value, increase res by 1
                if (prefXOR == k)
                    res++;
            }
        }
        return res;
    }

}
```
### Complexity Analysis
- **Time Complexity :** `O(n ^ 2)` 
- **Space Complexity :** `O(1)`


## [Efficient Approach] Using Hash Map and Prefix Sum – O(n) Time and O(n) Space

```
The idea is to use the properties of XOR. Let’s denote the XOR of all elements in the range [0, i] as A, the XOR of all elements in the range [i+1, j] as B, and the XOR of all elements in the range [0, j] as C. 

From the properties of XOR: C = A ⊕ B
This implies: A = C ⊕ B

Now if we know the value of C (the XOR of the prefix up to index j) and have B (the target XOR value k), we can determine A (the XOR of the prefix up to index i) using:  A = C ⊕ B

Using this relation, if a prefix sum A has already been seen earlier, it means there exists a subarray ending at j whose XOR is equal to k (B).
```

### Approach

1. Use a hash map to store the frequency of prefix XOR values encountered so far.
2. Traverse through the array and for each element at index i:
    * Update prefXOR = prefXOR ⊕ arr[i].
    * Check if A = prefXOR ⊕ k exists in the hash map. If it does, add the value of the hash map entry to result as it gives the count of subarrays ending at the current index that have XOR equal to k.
    * If prefXOR is equal to k, increment the result directly, as this indicates the subarray arr[0, i] has XOR equal to k.
    * Update the hash map to include the current prefXOR value.

### Code
```java
class Class{
    // Function to find the count of subarrays of arr with XOR value equals to k
    int subarrayXor(int[] arr, int k) {
        int res = 0;

        // Create map that stores number of prefix arrays corresponding to a XOR value
        HashMap<Integer, Integer> mp = new HashMap<>();

        int prefXOR = 0;

        for (int val : arr) {
          
            // Find XOR of current prefix
            prefXOR ^= val;

            // If prefXOR ^ k exists in mp then there is a subarray
            // ending at i with XOR equal to k
            res += mp.getOrDefault(prefXOR ^ k, 0);

            // If this prefix subarray has XOR equal to k
            if (prefXOR == k)
                res++;

            // Add the XOR of this subarray to the map
            mp.put(prefXOR, mp.getOrDefault(prefXOR, 0) + 1);
        }

        // Return total count of subarrays having XOR of
        // elements as given value k
        return res;
    }
}
```

### Complexity Analysis
- **Time Complexity :** `O(n)` 
- **Space Complexity :** `O(n)`


