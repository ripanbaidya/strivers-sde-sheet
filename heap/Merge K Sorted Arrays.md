# [Merge k Sorted Arrays](https://www.naukri.com/code360/problems/merge-k-sorted-arrays_975379?leftPanelTabValue=PROBLEM)

| Difficulty | Topic        | Companies | Resources   |
| ---------- | ------------ | --------- | ----------- |
| Medium     |  Heap        |           | [Video](#)  |
|            | 		        |  			| [Article](#)|

## Description
You have been given ‘K’ different arrays/lists, which are sorted individually (in ascending order). You need to merge all the given arrays/list such that the output array/list should be sorted in ascending order.

```
Sample Input 1:
1
2
3 
3 5 9 
4 
1 2 3 8   
Sample Output 1:
1 2 3 3 5 8 9 
Explanation of Sample Input 1:
After merging the two given arrays/lists [3, 5, 9] and [ 1, 2, 3, 8], the output sorted array will be [1, 2, 3, 3, 5, 8, 9].
Sample Input 2:
1
4
3
1 5 9
2
45 90
5
2 6 78 100 234
1
0
Sample Output 2:
0 1 2 5 6 9 45 78 90 100 234
Explanation of Sample Input 2 :
After merging the given arrays/lists [1, 5, 9], [45, 90], [2, 6, 78, 100, 234] and [0], the output sorted array will be [0, 1, 2, 5, 6, 9, 45, 78, 90, 100, 234].
```
**Constraints :**
- 1 <= T <= 5
- 1 <= K <= 5
- 1 <= N <= 20
- -10^5 <= DATA <= 10^5


## [Naive Approach]

### Code
```java
public class Solution {
	public static ArrayList<Integer> mergeKSortedArrays(ArrayList<ArrayList<Integer>> kArrays, int k){
		ArrayList<Integer> result = new ArrayList<>();

		for(ArrayList<Integer> list : kArrays){
			for(Integer el : list){
				result.add(el);
			}
		}

		Collections.sort(result);
		return result; 	
	}
}
```

### Complexity Analysis
- **Time Complexity:** `O(n * log n)`
- **Space Complexity** `O(n)`


## [Expected Approach]

### Code
```java
class HeapNode{
	public int value;
	public int arrayIndex;
	public int elementIndex;

	public HeapNode(int value, int arrayIndex, int elementIndex){
		this.value = value;
		this.arrayIndex = arrayIndex;
		this.elementIndex = elementIndex;
	}
}
public class Solution {
	public static ArrayList<Integer> 
		mergeKSortedArrays(ArrayList<ArrayList<Integer>> kArrays, int k){

		ArrayList<Integer> result = new ArrayList<>(); // result		
		PriorityQueue<HeapNode> minHeap = new PriorityQueue<>((a, b) -> a.value - b.value);

		// insert the first element of each array into the heap
		for(int i = 0; i < k; i ++){
			if(kArrays.get(i).size() > 0){
				minHeap.offer(new HeapNode(kArrays.get(i).get(0), i, 0));
			}
		}
		
		
		while(!minHeap.isEmpty()){
			HeapNode node = minHeap.poll();

			result.add(node.value); // add the first value
			if(node.elementIndex + 1 < kArrays.get(node.arrayIndex).size()){
				minHeap.offer(
					new HeapNode(
						kArrays.get(node.arrayIndex).get(node.elementIndex +1),
						node.arrayIndex,
						node.elementIndex + 1
						)
					);
			}
		}
		return result;
	}
}
```

### Complexity Analysis
- **Time Complexity:** `O(n * log k)`
- **Space Complexity** `O(n + k)`
