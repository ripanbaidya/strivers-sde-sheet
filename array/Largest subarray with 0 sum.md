# [Largest subarray with 0 sum](https://www.geeksforgeeks.org/problems/largest-subarray-with-0-sum/1)

| Difficulty | Topics       | Companies     | Resources                                             |
| ---------- | ------------ | ------------- | ------------------------------------------------------|
| **Medium** | Array        | **Amazon**    | [Video](https://youtu.be/xmguZ6GbatA)                 |
|            | Hash Table   | **Microsoft** | [Article](https://www.geeksforgeeks.org/find-the-largest-subarray-with-0-sum/)|

## Description
Ninja is given an array `Arr` of size `N`. You have to help him find the length of the longest subarray of `Arr` whose sum is `0`. You must return the length of the longest subarray whose sum is `0`.

**Examples**
```
Example 1:
Input: arr[] = [15, -2, 2, -8, 1, 7, 10, 23]
Output: 5
Explanation: The largest subarray with a sum of 0 is [-2, 2, -8, 1, 7].

Example 2:
Input: arr[] = [2, 10, 4]
Output: 0
Explanation: There is no subarray with a sum of 0.

Example 3:
Input: arr[] = [1, 0, -4, 3, 1, 0]
Output: 5
Explanation: The subarray is [0, -4, 3, 1, 0].
```

**Constraints:**

- `1 <= N <= 10^5`
- `-10^9 <= Arr[i] <= 10^9`
- The sum of `N` over all test cases is less than or equal to `10^5`.
- Time Limit: 1 sec.

## [Naive Approach] Iterating over all subarrays – O(n^2) Time and O(1) Space

Generate all sub-arrays one by one and check the sum of each subarray. If the sum of the current subarray is equal to zero then update the maximum length accordingly. After iterating over all the subarrays, return the maximum length.

### Code
``` java
class Solution {
    int maxLen(int arr[]) {
        int n = arr.length;
        int maxi = 0;
        
        for(int i = 0; i < n; i ++){
            int sum = 0;
            for(int j = i; j < n; j ++){
                sum += arr[j];
                
                if(sum == 0){
                    maxi = Math.max(j-i+1, maxi);
                }
            }
        }
        return maxi;
    }
}
```
### Complexity Analysis
- **Time Complexity :** `O(n ^ 2)` 
- **Space Complexity :** `O(1)`


## [Expected Approach] Using Hashmap and Prefix Sum – O(n) Time and O(n) Space

```
The idea is based on the observation that for two different indices i and j (where j > i) if the prefix sums Si and Sj are equal, it means that the sum of the elements between indices i+1 and j is zero. This is because:

Sj – Si = arr[i+1] + arr[i+2] + …… + arr[j]
If Si = Sj, then: Sj – Si = 0
This means the subarray from i+1 to j has a sum of zero.

Illustration:
Consider the array arr = {5, 2, -1, 1, 4}. Calculate the prefix sums:

S0 = 5
S1 = 5 + 2 = 7
S2 = 5 + 2 – 1 = 6
S3 = 5 + 2 – 1 + 1 = 7
Here, S1 = S3 = 7. This equality tells us that the subarray from index 2 to 3 (subarray [-1, 1]) sums to zero.
``` 

* Use a hashmap (or dictionary) to store the first occurrence of each prefix sum.
* Iterate over each element of array:
    * Check if the current prefix sum has been seen before, it means a subarray with zero sum exists between the previous index (where this prefix sum was first seen) and the current index.
    * Keep track of the maximum length of any zero-sum subarray found.

### Code
```java
class Solution {
    int maxLen(int arr[]) {
        int n = arr.length;
        int maxi = 0, sum = 0;
        HashMap<Integer, Integer> mp = new HashMap<>(); // <prefixSum, index>
        
        for(int i = 0; i < n; i ++){
            sum += arr[i];
                
            if(sum == 0){
                maxi = i + 1;
            } else {
                if(mp.containsKey(sum)){
                    maxi = Math.max(i - mp.get(sum), maxi);
                } else {
                    mp.put(sum, i);
                }
            }
        }
        return maxi;
    }
}
```
### Complexity Analysis
- **Time Complexity :** `O(n)` 
- **Space Complexity :** `O(n)`