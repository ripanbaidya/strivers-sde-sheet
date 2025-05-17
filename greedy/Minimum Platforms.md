# [Minimum Platforms](https://www.geeksforgeeks.org/problems/minimum-platforms-1587115620/1)

| Difficulty | Topics               | Companies | Video |
| ---------- | -------------------- | --------- | ----- |
| **Medium** | Greedy, Algorithms   | MAANG     | [Video](https://youtu.be/AsGzwR_FWok) |
|            | Array, Sorting       |           | [Article](https://www.geeksforgeeks.org/minimum-number-platforms-required-railwaybus-station/) |
|            | Binary Search        |           |

## Description
Given two arrays, arr[] and dep[], that represent the arrival and departure times of trains respectively, the task is to find the minimum number of platforms required so that no train waits.

**Examples:** 
```
Example 1:
Input: arr[] = [900, 940, 950, 1100, 1500, 1800], dep[] = [910, 1200, 1120, 1130, 1900, 2000]
Output: 3 
Explanation: There are three trains during the time 9:40 to 12:00. So we need a minimum of 3 platforms.

Example 2:
Input: arr[] = [1,  5], dep[] = [3, 7] 
Output: 1 
Explanation:  All train times are mutually exclusive. So we need only one platform
```

## [Naive Approach] Using Two Nested Loops – O(n^2) time and O(1) space
```
The idea is to iterate through each train and for that train, check how many other trains have overlappingtimings with it – where current train’s arrival time falls between the other train’s arrival and departure times. We keep track of this count for each train and continuously update our answer with the maximum count found.
```

### Code
```java
class Solution {
    // Function to find the minimum number of platforms required
    int findMinimumPlatforms(int[] arrivalTimes, int[] departureTimes) {
        int numberOfTrains = arrivalTimes.length;
        int maxPlatformsRequired = 0;

        // Traverse each train's arrival time and find overlapping trains
        for (int currentTrain = 0; currentTrain < numberOfTrains; currentTrain++) {
            int platformsNeeded = 1; // At least one platform for the current train
            for (int otherTrain = 0; otherTrain < numberOfTrains; otherTrain++) {
                if (currentTrain != otherTrain) {
                    // Check for overlap between trains
                    if (arrivalTimes[currentTrain] >= arrivalTimes[otherTrain] &&
                        departureTimes[otherTrain] >= arrivalTimes[currentTrain]) {
                        platformsNeeded++;
                    }
                }
            }
            // Update the maximum platforms required
            maxPlatformsRequired = Math.max(platformsNeeded, maxPlatformsRequired);
        }
        return maxPlatformsRequired;
    }
}
```

### Complexity Analysis

- Time complexity : `O (n^2)`, where n is the number of nodes in the tree.
- Space complexity : `O(1)`


## [Expected Approach] Using Sorting and Two Pointers – O(n log(n)) time and O(1) space
This approach uses sorting and two-pointer to reduce the complexity. First, we sort the arrival and departure times of all trains. Then, using two pointers, we traverse through both arrays. 

### Algorithm

* Sort the arrival and departure times so we can process train timings in order.
* Initialize two pointers:
    * One for tracking arrivals (i = 0).
    * One for tracking departures (j = 0).
* Iterate through the arrival times:
    * If the current train arrives before or at the departure of an earlier train, allocate a new platform (cnt++).
    * Otherwise, if the arrival time is greater than the departure time, it means a train has left, freeing up a platform (cnt--), and move the departure pointer forward (j++).
* Update the maximum number of platforms required after each step.
* Continue this process until all trains are processed.

### Code
```java
class Solution {
    /**
     * Finds the minimum number of platforms required at the railway station
     * such that no train waits.
     */
    static int findPlatform(int[] arrivalTimes, int[] departureTimes) {
        // Sort the arrival and departure times
        Arrays.sort(arrivalTimes);
        Arrays.sort(departureTimes);

        int trainCount = 0; // the number of trains that have not departed yet
        int maxPlatformsNeeded = 0; // the maximum number of platforms needed
        int trainIndex = 0; // the current index in the arrival array
        int departureIndex = 0; // the current index in the departure array

        // Process the trains
        while (trainIndex < arrivalTimes.length) {
            // A new train arrives
            if (arrivalTimes[trainIndex] <= departureTimes[departureIndex]) {
                trainCount++;
                trainIndex++;
            } else { // a train leaves
                trainCount--;
                departureIndex++;
            }

            // Update the maximum number of platforms needed
            maxPlatformsNeeded = Math.max(trainCount, maxPlatformsNeeded);
        }

        return maxPlatformsNeeded;
    }
}
```

### Complexity Analysis

- Time complexity : `O (n log n)` + `O(2n)`, where n is the number of nodes in the tree.
- Space complexity : `O(1)`

