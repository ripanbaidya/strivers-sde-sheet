# [Job Sequencing Problem](https://www.geeksforgeeks.org/problems/job-sequencing-problem-1587115620/1)

| Difficulty | Topics         | Companies | Video   |
| ---------- | -------------- | --------- | ------- |
| **Medium** | Greedy         | Flipkart  | [Video](https://youtu.be/QbwltemZbRg?si=duEbM6PM82d0DsuY) |
|            | Dp, Algorithms | Microsoft | [Article](https://www.geeksforgeeks.org/job-sequencing-problem/) |

## Description
You are given two arrays: deadline[], and profit[], which represent a set of jobs, where each job is associated with a deadline, and a profit. Each job takes 1 unit of time to complete, and only one job can be scheduled at a time. You will earn the profit associated with a job only if it is completed by its deadline.

Your task is to find:

1. The maximum number of jobs that can be completed within their deadlines.
2. The total maximum profit earned by completing those jobs.

**Examples :**

```
Example 1:
Input: deadline[] = [4, 1, 1, 1], profit[] = [20, 10, 40, 30]
Output: [2, 60]
Explanation: Job1 and Job3 can be done with maximum profit of 60 (20+40).

Example 2:
Input: deadline[] = [2, 1, 2, 1, 1], profit[] = [100, 19, 27, 25, 15]
Output: [2, 127]
Explanation: Job1 and Job3 can be done with maximum profit of 127 (100+27).

Example 3:
Input: deadline[] = [3, 1, 2, 2], profit[] = [50, 10, 20, 30]
Output: [3, 100]
Explanation: Job1, Job3 and Job4 can be completed with a maximum profit of 100 (50 + 20 + 30).
```

**Constraints:**

- 1 ≤ number of jobs ≤ 10^5
- All arrays have equal length
- 1 ≤ id[i], deadline[i] ≤ number of jobs
- 1 ≤ profit[i] ≤ 500

## Using Greedy Algorithm

The strategy to maximize profit should be to pick up jobs that offer higher profits. Hence we should sort the jobs in `descending order of profit`. Now say if a job has a deadline of 4 we can perform it anytime between day 1-4, but it is preferable to perform the job on its last day. This leaves enough empty slots on the previous days to perform other jobs.

**Basic Outline of the approach:**
* Sort the jobs in descending order of profit. 
* If the maximum deadline is x, make an array of size x 1.Each array index is set to -1 initially as no jobs have been performed yet.
* For every job check if it can be performed on that day to its all previous day.
* If possible mark that index with the job id and add the profit to our answer. 
* If not possible, loop through the previous indexes until an empty slot is found.

**Dry Run**

![](https://lh4.googleusercontent.com/oPnEMwtHivvA7Uy36G1lSQSraQ6xJ__THJJ3XiLL3mP--VgNerwFaPZgfXWbS3WDFYn2EKL8WiG3VDu3fAtB5Ii3ZFiT5-Ln8XmM1_zwH0Q7sTg_28NYWBDOP07_MQ)

### Code
```java
class Job {
    int jobId;
    int deadline;
    int profit;

    public Job(int jobId, int deadline, int profit) {
        this.jobId = jobId;
        this.deadline = deadline;
        this.profit = profit;
    }
}
/**
 * Job Sequencing Problem
 * 
 * Given a set of jobs with deadlines and profits, find the maximum profit that
 * can be obtained by completing the jobs in a given deadline.
 */
class JobSequencing {
    public ArrayList<Integer> jobSequencing(int[] jobIds, int[] deadlines, int[] profits) {
        ArrayList<Job> jobs = new ArrayList<>();
        int maxDeadline = 0;
        int maxProfit = 0, maxNumJobs = 0; // maximum profit, maximum number of jobs
        
        // find the maximum deadline
        for (int deadline : deadlines) {
            maxDeadline = Math.max(maxDeadline, deadline);
        }
        
        // create a hash array with size (n+1) to mark which job has done
        int[] jobStatus = new int[maxDeadline + 1];
        Arrays.fill(jobStatus, -1); // fill the hash array by -1.
        
        // put every element into Job(C)
        for (int i = 0; i < jobIds.length; i++) {
            jobs.add(new Job(jobIds[i], deadlines[i], profits[i]));
        }
        // Sorting Jobs based on profit in descending order
        Collections.sort(jobs, (a, b) -> b.profit - a.profit);
        
        // calculating maximum profit and maximum jobs
        for (int i = 0; i < jobs.size(); i++) {
            for (int j = jobs.get(i).deadline; j > 0; j--) {
                if (jobStatus[j] == -1) {
                    maxNumJobs++;
                    maxProfit += jobs.get(i).profit;
                    jobStatus[j] = jobs.get(i).jobId;
                    break;
                }
            }
        }
        ArrayList<Integer> result = new ArrayList<>();
        result.add(maxNumJobs);
        result.add(maxProfit);
        
        return result;
    }
}
```
### Complexity Analysis

- **Time complexity**:
  - Sorting : `O(n log n)`
  - Iterating through the jobs : `O(n)`
  - Iterating through the jobStatus array : `O(n ^ 2)`

- - So, The total time complexity is `O(n ^ 2)`.

- **Space complexity**: `O(n)` due to the use of the `jobStatus` array.

