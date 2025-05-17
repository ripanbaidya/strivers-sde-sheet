# [N meetings in one room](https://www.geeksforgeeks.org/problems/n-meetings-in-one-room-1587115620/1)

| Difficulty | Topics         | Companies           | Video |
| ---------- | -------------- | ------------------- | ----- |
| **Medium** | Greedy         | Flipkart            | [Video](https://youtu.be/II6ziNnub1Q) |
|            | Algorithms     | Microsoft, Amazon   | [Article](https://www.geeksforgeeks.org/find-maximum-meetings-in-one-room/)|

## Description
You are given the timings of `n` meetings in the form of two arrays `start[]` and `end[]`, where `start[i]` represents the start time of meeting `i` and `end[i]` represents the end time of meeting `i`. Your task is to determine the **maximum number of meetings** that can be accommodated in a single meeting room, given that only one meeting can be held at any particular time.

**Note:**  
The start time of one chosen meeting cannot be equal to the end time of another chosen meeting (i.e., back-to-back meetings are allowed, but overlapping meetings are not).  

```
Example 1:
Input:
N = 6
S = {1,3,0,5,8,5}
F = {2,4,6,7,9,9} 
Output:
{1,2,4,5}
Explanation:
We can attend the 1st meeting from (1 to 2), then the 2nd meeting from (3 to 4), then the 4th meeting from (5 to 7), and the last meeting we can attend is the 5th from (8 to 9). It can be shown that this is the maximum number of meetings we can attend.
```

```
Example 2:
Input:
N = 1
S = {3}
F = {7}
Output:
{1}
Explanation:
Since there is only one meeting, we can attend the meeting.
```

**Your Task:**

You don't need to read input or print anything. Your task is to complete the function maxMeetings() which takes the arrays S, F, and its size N as inputs and returns the meeting numbers we can attend in sorted order.

### **Constraints:**

- 1 <= N <= 105
- 0 <= S[i] <= F[i] <= 109
- Sum of N over all test cases doesn't exceeds 106


## Using Greedy Algorithm

### Initial Thought Process
Say if you have two meetings, one which gets over early and another which gets over late. Which one should we choose?  If our meeting lasts longer the room stays occupied and we lose our time. On the other hand, if we choose a meeting that finishes early we can accommodate more meetings. Hence we should choose meetings that end early and utilize the remaining time for more meetings.

### Approach: 

To proceed we need a `vector/ArrayList` of three quantities (depends on Programming Requirement): the `starting time`, `ending time`, `meeting number`. Sort this data structure in `ascending order` of `end time`. 

We need a variable to store the `answer`. Initially, the answer is 1 because the first meeting can always be performed. Make another variable, say `limit` that keeps track of the ending time of the meeting that was last performed. Initially set limit as the end time of the first meeting.

Start iterating from the second meeting. At every position we have two possibilities:

* If the start time of a meeting is  strictly greater than limit we can perform the meeting. Update the answer.Our new limit is the ending time of the current meeting  since it was last performed.Also update limit.  

* If the start time is less than or equal to limit  ,skip and move ahead. 


***Let's have a dry run by taking the following example.***
```
N = 6,  start[] = {1,3,0,5,8,5}, end[] =  {2,4,5,7,9,9}
```
Initially set `answer =[1]`, `limit = 2`.

* For Position 2 
    - Start time of meeting no. 2 = 3 > limit. Update answer and limit.
    - Answer = [1, 2], limit = 4.

* For Position 3 -
    - Start time of meeting no. 3 = 0 < limit.Nothing is changed.

* For Position 4 -

    - Start time of meeting no. 4 = 5 > limit. Update answer and limit.
    - Answer = [1,2,4], limit = 7.

* For Position 5 -
    - Start time of meeting no. 5 = 8 > limit.Update answer and limit.
    - Answer = [1,2,4,5], limit = 9.

* For Position 6 -
    - Start time of meeting no. 6 = 8 < limit.Nothing is changed.
    - **Final answer**  =  `[1,2,4,5]`

### Code
```java
class Meeting {
    int startTime, endTime;

    public Meeting(int startTime, int endTime) {
        this.startTime = startTime;
        this.endTime = endTime;
    }
}

class Solution {
    public int maxMeetings(int[] startTimes, int[] endTimes) {
        int numMeetings = startTimes.length;

        if (numMeetings == 0) return 0;

        List<Meeting> meetings = new ArrayList<>();
        for (int i = 0; i < numMeetings; i++) {
            meetings.add(new Meeting(startTimes[i], endTimes[i]));
        }

        // Sort meetings by end time in ascending order
        meetings.sort((m1, m2) -> m1.endTime - m2.endTime);

        int maxMeetingsCount = 1; // At least one meeting can be attended
        int lastMeetingEndTime = meetings.get(0).endTime;

        for (int i = 1; i < numMeetings; i++) {
            if (meetings.get(i).startTime > lastMeetingEndTime) {
                maxMeetingsCount++;
                lastMeetingEndTime = meetings.get(i).endTime;
            }
        }

        return maxMeetingsCount;
    }
}
```

### Complexity Analysis

- ***Time Complexity*** : `O(n) + O(n) + O(n log n)` ~ `O(n log n)`
- ***Space Complexity*** : `O(n)`
