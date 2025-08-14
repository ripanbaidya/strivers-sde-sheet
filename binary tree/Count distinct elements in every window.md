# [Count distinct elements in every window](https://www.geeksforgeeks.org/problems/count-distinct-elements-in-every-window/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=practice_card)

| Difficulty | Topic | Companies | Resources   |
| ---------- | ----- | --------- | ----------- |
| **Medium** | BST   |           | [Video](https://www.geeksforgeeks.org/count-distinct-elements-in-every-window-of-size-k/)   |
|            |       |           | [Article](https://www.geeksforgeeks.org/count-distinct-elements-in-every-window-of-size-k/) |

## Description
Given an array arr[] of size n and an integer k, return the count of distinct numbers in all windows of size k. 

**Examples**

```
Input: arr[] = [1, 2, 1, 3, 4, 2, 3], k = 4
Output: [3, 4, 4, 3]
Explanation: First window is [1, 2, 1, 3], count of distinct numbers is 3.
                      Second window is [2, 1, 3, 4] count of distinct numbers is 4.
                      Third window is [1, 3, 4, 2] count of distinct numbers is 4.
                      Fourth window is [3, 4, 2, 3] count of distinct numbers is 3.

Input: arr[] = [4, 1, 1], k = 2
Output: [2, 1]
Explanation: First window is [4, 1], count of distinct numbers is 2.
                      Second window is [1, 1], count of distinct numbers is 1.
```

**Constraints:**

- 1 <= k <= arr.size() <= 105
- 1 <= arr[i] <= 105


## [Naive Approach] Traversing all windows of size k - O(n * k) Time and O(1) Space
Traverse the given array considering every window of size k in it and keeping a count on the distinct elements of the window.

### Code 
```java
class Solution {
    ArrayList<Integer> countDistinct(int[] arr, int windowSize) {
        int arrayLength = arr.length;
        ArrayList<Integer> distinctCountList = new ArrayList<>();
        Map<Integer, Integer> frequencyMap = new HashMap<>();

        // Initialize the frequency map for the first window
        for (int i = 0; i < windowSize; i++) {
            frequencyMap.put(arr[i], frequencyMap.getOrDefault(arr[i], 0) + 1);
        }
        distinctCountList.add(frequencyMap.size());

        // Slide the window across the array
        for (int i = windowSize; i < arrayLength; i++) {
            // Add the new element of the window
            frequencyMap.put(arr[i], frequencyMap.getOrDefault(arr[i], 0) + 1);

            // Remove the element that is slid out of the window
            int elementToRemove = arr[i - windowSize];
            frequencyMap.put(elementToRemove, frequencyMap.get(elementToRemove) - 1);

            // Remove the element from the map if its frequency is 0
            if (frequencyMap.get(elementToRemove) == 0) {
                frequencyMap.remove(elementToRemove);
            }

            // Add the current window's distinct count to the result
            distinctCountList.add(frequencyMap.size());
        }
        return distinctCountList;
    }
}
```

### Complexity Analysis

- **Time Complexiy** : `O(n * k)`
- **Space Complexity** : `O(1)`


## [Expected Approach] Sliding Window Technique - O(n) Time and O(k) Space

The idea is to use Sliding Window Technique and count the number of distinct element in the current window using the count of previous window. Maintain a hash map or dictionary to store the frequency of elements of the current window and the count of distinct elements in the window will be equal to the size of the hash map.

Store the frequency of first k elements in the hash map. Now start iterating from index = k, 

* increase the frequency of arr[k] in the hash map.
* decrease the frequency of arr[i - k] in the hash map. If frequency of arr[i - k] becomes 0, remove arr[i] from the hash map.
* insert size of hash map into the resultant array.

### Code
```java
class Solution {
    ArrayList<Integer> countDistinct(int[] arr, int windowSize) {
        int arrayLength = arr.length;
        ArrayList<Integer> distinctCountList = new ArrayList<>();
        Map<Integer, Integer> elementFrequencyMap = new HashMap<>();
        
        // Initialize the frequency map for the first window
        for (int i = 0; i < windowSize; i++) {
            elementFrequencyMap.put(arr[i], elementFrequencyMap.getOrDefault(arr[i], 0) + 1);
        }
        distinctCountList.add(elementFrequencyMap.size());
        
        // Slide the window across the array
        for (int i = windowSize; i < arrayLength; i++) {
            // Add the new element of the window
            elementFrequencyMap.put(arr[i], elementFrequencyMap.getOrDefault(arr[i], 0) + 1);
            
            // Remove the element that is slid out of the window
            int elementToRemove = arr[i - windowSize];
            elementFrequencyMap.put(elementToRemove, elementFrequencyMap.get(elementToRemove) - 1);
            
            // Remove the element from the map if its frequency is 0
            if (elementFrequencyMap.get(elementToRemove) == 0) {
                elementFrequencyMap.remove(elementToRemove);
            }
            
            // Add the current window's distinct count to the result
            distinctCountList.add(elementFrequencyMap.size());
        }
        return distinctCountList;
    }
}
```

### Complexity Analysis

- **Time Complexiy** : `O(n)`
- **Space Complexity** : `O(k)`