| Difficulty | Topic               | Companies | Resources   |
| ---------- | ------------------- | --------- | ----------- |
| **Medium** | Array, Prefix Sum   | Amazon    | [Video](https://youtu.be/frf7qxiN2qU)   |
|            |                     |           | [Article](https://www.geeksforgeeks.org/dsa/longest-sub-array-sum-k/) |

## Description

Given an array arr[] containing integers and an integer k, your task is to find the length of the longest subarray where the sum of its elements is equal to the given value k. If there is no subarray with sum equal to k, return 0.

**Example 1**
```
Input: arr[] = [10, 5, 2, 7, 1, -10], k = 15
Output: 6
Explanation: Subarrays with sum = 15 are [5, 2, 7, 1], [10, 5] and [10, 5, 2, 7, 1, -10]. The length of the longest subarray with a sum of 15 is 6.
```

**Example 2**
```
Input: arr[] = [-5, 8, -14, 2, 4, 12], k = -5
Output: 5
Explanation: Only subarray with sum = -5 is [-5, 8, -14, 2, 4] of length 5.
```

**Constraints:**

- 1 ≤ arr.size() ≤ 105
- -104 ≤ arr[i] ≤ 104
- -109 ≤ k ≤ 109

## [Naive Approach] Using Nested Loop - O(n^2) Time and O(1) Space 
 
The idea is to check the sum of all the subarrays and return the length of the longest subarray having the sum k.

### Code
```java
class Solution {
    public int longestSubarray(int[] arr, int k) {
        int n = arr.length;
        int longest = 0;
        
        // generate all subarray
        for(int i = 0; i < n; i ++){
            // Sum of subarray from i to j
            int sum = 0;
            for(int j = i; j < n; j ++){
                sum += arr[j];
                
                // If subarray sum is equal to k
                if(sum == k){
                    longest = Math.max(longest, j-i+1);
                }
            }
        }
        return longest;
    }
}
```

### Complexity Analysis

- **Time Complexiy** : `O(n^2)`
- **Space Complexity** : `O(1)`


## [Expected Approach] Using Hash Map and Prefix Sum - O(n) Time and O(n) Space
 

```
The idea is based on the fact that if Sj - Si = k (where Si and Sj are prefix sums till index i and j respectively, and  i < j), then the subarray between i+1 to j has sum equal to k. 
For example, arr[] = [5, 2, -3, 4, 7] and k = 3.  The value of S3 - S0= 3,  it means the subarray from index 1 to 3 has sum equals to 3. 

So we mainly compute prefix sums in the array and store these prefix sums in a hash table. And check if current prefix sum - k is already present. If current prefix sum - k is present in the hash table and is mapped to index j, then subarray from j to current index has sum equal to k.
```
Main Consideration

1. If we have the whole prefix having sum equal to k, we should prefer it as it would be the longest possible till that point.

2. If there are multiple occurrences of a prefixSum, we must store index of the earliest occurrence of prefixSum because we need to find the longest subarray.

### Code
```java
class Solution {
    public int longestSubarray(int[] nums, int k) {
        int n = nums.length;

        // {prefSum, index}
        Map<Integer, Integer> mp = new HashMap<>();
        int prefSum = 0;
        int longest = 0;
        
        for(int i = 0; i < n; i ++){
            // compute the prefix Sum
            prefSum += nums[i];

            // when prefix sum equals k
            if(prefSum == k){
                longest = Math.max(longest, i+1);
            }

            // remainging element we are looking in the map
            int remaining = prefSum - k;

            if(mp.containsKey(remaining)){
                // update the length
                longest = Math.max(longest, i - mp.get(remaining));
            }

            // Store only first occurrence index of prefSum
            if(!mp.containsKey(prefSum))
                mp.put(prefSum, i);
        }

        return longest;
    }
}
```

### Complexity Analysis

- **Time Complexiy** : `O(n)`
- **Space Complexity** : `O(n)`