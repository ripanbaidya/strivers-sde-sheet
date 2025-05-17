# [Fractional Knapsack](https://www.geeksforgeeks.org/problems/fractional-knapsack-1587115620/1)

| Difficulty | Topics         | Companies | Video |
| ---------- | -------------- | --------- | ----- |
| **Easy**   | Greedy         | Microsoft | [Video](https://youtu.be/F_DDzYnxO14) |
|            | Algorithms     |           | [Article](https://takeuforward.org/data-structure/fractional-knapsack-problem-greedy-approach/) |

## Description

Given two arrays, val[] and wt[], representing the values and weights of items, and an integer capacity representing the maximum weight a knapsack can hold, determine the maximum total value that can be achieved by putting items in the knapsack. 

You are allowed to break items into fractions if necessary.
Return the maximum value as a double, rounded to 6 decimal places.

**Examples**

```
Example1 :
Input: val[] = [60, 100, 120], wt[] = [10, 20, 30], capacity = 50
Output: 240.000000
Explanation: Take the item with value 60 and weight 10, value 100 and weight 20 and split the third item with value 120 and weight 30, to fit it into weight 20. so it becomes (120/30)*20=80, so the total value becomes 60+100+80.0=240.0 Thus, total maximum value of item we can have is 240.00 from the given capacity of sack. 
```

```
Example 2:
Input: val[] = [60, 100], wt[] = [10, 20], capacity = 50
Output: 160.000000
Explanation: Take both the items completely, without breaking. Total maximum value of item we can have is 160.00 from the given capacity of sack.
```

**Constraints:**
- 1 <= val.size=wt.size <= 105
- 1 <= capacity <= 109
- 1 <= val[i], wt[i] <= 104


## Using Greedy Algorithm

The greedy method to maximize our answer will be to pick up the items with higher values. Since it is possible to break the items as well we should focus on picking up items having higher value /weight first. To achieve this, items should be sorted in decreasing order with respect to their value /weight. Once the items are sorted we can iterate. Pick up items with weight lesser than or equal to the current capacity of the knapsack. In the end, if the weight of an item becomes more than what we can carry, break the item into smaller units. Calculate its value according to our current capacity and add this new value to our answer.

Let's understand with an example:-

```
N = 3, W = 50, values[] = {100,60,120}, weight[] = {20,10,30}.

```
The value/weight of item 1 is (100/20) = 5,for item 2 is (60/10) = 6 and for item 3 is (120/30) = 4.<br>
Sorting them in decreasing order of value/weight we have 

![](https://lh6.googleusercontent.com/MykKGYWNZV6LI1wrcKjxKS2BKfqj5yrjXp9N8RwCBi5_U151TwIIqrBIhQ47we4RmgJwgwKBpc9bVMqq2XpjUrR3atXB_FDjrAhCkk7u7ivLItr_lQmYiSpZccnjQTOgW1KN0QIe)

- Initially capacity of bag(W)  = 50, value = 0
- Item 1 has a weight of 10, we can pick it up. 
- Current weight = 50 - 10 = 40 ,Current value = 60.00
- Item 2  has a weight of 20 , we can pick it up. 
- Current weight = 40 - 20 = 20 ,Current value = 60.00 + 100.00 = 160.00
- Item 3 has a weight of 30 , but current knapsack capacity is 20.Only a fraction of it is chosen.
- Current weight = 20 - 20 = 0 ,Final value = 160.00 + (120/30 )*20 = 240.00

### Code
```java
class Item {
    int value;
    int weight;

    public Item(int value, int weight) {
        this.value = value;
        this.weight = weight;
    }
}

// Comparator to sort items based on their value-to-weight ratio in descending order
class ItemComparator implements Comparator<Item> {
    @Override
    public int compare(Item item1, Item item2) {
        double ratio1 = (double) item1.value / item1.weight;
        double ratio2 = (double) item2.value / item2.weight;
        return Double.compare(ratio2, ratio1);
    }
}

class FractionalKnapsack {
    // Function to get the maximum total value in the knapsack
    double getMaxTotalValue(List<Integer> values, List<Integer> weights, int capacity) {
        assert values.size() == weights.size();

        int numItems = values.size();
        List<Item> items = new ArrayList<>();
        for (int i = 0; i < numItems; i++) {
            items.add(new Item(values.get(i), weights.get(i)));
        }

        // Sort the items in descending order based on their value-to-weight ratio
        Collections.sort(items, new ItemComparator());

        double totalValue = 0.0;

        for (Item item : items) {
            if (item.weight <= capacity) {
                totalValue += item.value;
                capacity -= item.weight;
            } else {
                double newValue = (double) item.value / item.weight * capacity;
                totalValue += newValue;
                break;
            }
        }

        return totalValue;
    }
}
```

### Complexity Analysis

- ***Time Complexity*** : `O(n log n + n). O(n log n)` to sort the items and `O(n)` to iterate through all the items for calculating the answer.
- ***Space Complexity*** : `O(1)`, no additional data structure has been used.
