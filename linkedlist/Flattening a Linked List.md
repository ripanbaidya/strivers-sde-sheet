# [Flattening a Linked List](https://www.geeksforgeeks.org/problems/flattening-a-linked-list/1)

| Difficulty | Topics       | Companies         | Video                                            |
| ---------- | ------------ | ----------------- | ------------------------------------------------ |
| **Medium** | LinkedList   |  Paytm, Flipkart  | [Video](https://youtu.be/ysytSSXpAI0)            |
|            | Recursion    | Amazon, Microsoft | [Article](https://www.geeksforgeeks.org/flattening-a-linked-list/)|

## Description

Given a linked list containing `n` head nodes where every node in the linked list contains two pointers:  
1. `next` points to the next node in the list.  
2. `bottom` pointer points to a sub-linked list where the current node is the head.  

Each of the sub-linked lists and the head nodes are sorted in ascending order based on their data.  

Your task is to flatten the linked list such that all the nodes appear in a single level while maintaining the sorted order.  

**Note:**  
1. `↓` represents the bottom pointer and `->` represents the next pointer.  
2. The flattened list will be printed using the bottom pointer instead of the next pointer.  


**Example 1:**  

**Input:**  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700192/Web/Other/blobid0_1722066129.png)

**Output:**  
`5->7->8->10->19->20->22->28->30->35->40->45->50`  

**Explanation:**  
- Bottom pointer of `5` is pointing to `7`.  
- Bottom pointer of `7` is pointing to `8`.  
- Bottom pointer of `8` is pointing to `10` and so on.  

**Example 2:**  

**Input:**  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700192/Web/Other/blobid1_1722066171.png)

**Output:**  
`5->7->8->10->19->20->22->28->30`  

**Explanation:**  
- Bottom pointer of `5` is pointing to `7`.  
- Bottom pointer of `7` is pointing to `8`.  
- Bottom pointer of `8` is pointing to `10` and so on.  

**Constraints:**  
- `0 <= n <= 100`  
- `1 <= number of nodes in sub-linked list (mi) <= 50`  
- `1 <= node->data <= 10^4`

## [Expected Approach] Flattening a Linked List by Merging lists recursively – O(n * n * m) Time and O(n) Space

### Intuition

The problem involves flattening a linked list where each node has two pointers: `next` (points to the next node in the main list) and `bottom` (points to a sub-linked list). Both the main list and sub-linked lists are sorted in ascending order. The goal is to merge all the sub-linked lists into a single sorted linked list using the `bottom` pointer.

The key idea is to recursively flatten the linked list by merging two lists at a time. Starting from the last node of the main list, we merge it with its `next` list, and then merge the result with the previous list, and so on, until the entire list is flattened.

### Approach

1. **Base Case**: If the current node is `null` or its `next` pointer is `null`, return the node itself (no further merging is needed).
2. **Recursive Flattening**: Recursively flatten the `next` list to ensure all sub-linked lists are merged.
3. **Merge Two Lists**: Use a helper function (`mergeTwoList`) to merge the current node's sub-linked list with the flattened `next` list. This function works similarly to merging two sorted linked lists.
4. **Return the Result**: After merging, return the head of the flattened list.

### Code
```java
class Solution {
    // Utility function to merge two sorted linked lists
    // using their bottom pointers
    static Node merge(Node head1, Node head2) {
      
        Node dummy = new Node(-1);

        // Tail points to the last result node to add new nodes to the result
        Node tail = dummy;

        // Iterate till either head1 or head2 does not reach null
        while (head1 != null && head2 != null) {
            if (head1.data <= head2.data) {
                // Append head1 to the result
                tail.bottom = head1;
                head1 = head1.bottom;
            } else {              
                // Append head2 to the result
                tail.bottom = head2;
                head2 = head2.bottom;
            }
            // Move tail pointer to the next node
            tail = tail.bottom;
        }

        // Append the remaining nodes of the non-null list
        if (head1 != null)
            tail.bottom = head1;
        else 
            tail.bottom = head2;
        
        return dummy.bottom;
    }

    // Function to flatten the linked list
    static Node flatten(Node root) {
        // Base Cases
        if (root == null || root.next == null)
            return root;

        // Recur for next list
        root.next = flatten(root.next);

        // Now merge the current and next list
        root = merge(root, root.next);

        // Return the root
        return root;
    }
}
```

### Complexity Analysis

- **Time Complexity**: `O(n * n * m)` – where n is the no of nodes in the main linked list and m is the no of nodes in a single sub-linked list. 

  * After adding the first 2 lists, the time taken will be O(m+m) = O(2m).
  * Then we will merge another list to above merged list -> time = O(2m + m) = O(3m).
  * We will keep merging lists to previously merged lists until all lists are merged.
  * Total time taken will be O(2m + 3m + 4m + …. n*m) = (2 + 3 + 4 + … + n) * m = O(n * n * m)

- **Auxiliary Space**: `O(n)`, the recursive functions will use a recursive stack of a size equivalent to a total number of nodes in the main linked list.