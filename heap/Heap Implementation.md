# Max Heap & Min Heap Implementation

| Difficulty | Topic            | Companies      | Resources   |
| ---------- | ---------------- | -------------- | ----------- |
| Medium     | Heap             |  Amazon        | [Video]()   |
|            | Priority Queue   | Microsoft      | [Article](https://www.geeksforgeeks.org/heap-implementation-in-java/) |


### Code
```java
class Heap {
    private List<Integer> heap = new ArrayList<>();

    /**
     * Inserts the specified element into this heap.
     *
     * @param element the element to insert
     */
    public void insert(int element) {
        heap.add(element);
        percolateUp(heap.size() - 1);
    }

    /**
     * Removes and returns the root element of this heap.
     *
     * @return the root element of this heap
     */
    public int delete() {
        int root = heap.get(0);
        heap.set(0, heap.remove(heap.size() - 1));
        percolateDown(0);
        return root;
    }

    /**
     * Returns the root element of this heap.
     *
     * @return the root element of this heap
     */
    public int peek() {
        return heap.get(0);
    }

    /**
     * Returns the size of this heap.
     *
     * @return the size of this heap
     */
    public int size() {
        return heap.size();
    }

    /**
     * Prints the elements of this heap.
     */
    public void print() {
        for (int element : heap) {
            System.out.print(element + " ");
        }
        System.out.println();
    }

    /**
     * Heapifies the specified subtree rooted at index {@code i}.
     *
     * @param i the index of the root of the subtree
     */
    private void percolateDown(int i) {
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        int largest = i;

        if (left < heap.size() && heap.get(left) > heap.get(largest)) {
            largest = left;
        }

        if (right < heap.size() && heap.get(right) > heap.get(largest)) {
            largest = right;
        }

        if (largest != i) {
            Collections.swap(heap, i, largest);
            percolateDown(largest);
        }
    }

    /**
     * Ensures that the heap property is maintained at index {@code i}.
     *
     * @param i the index of the element to ensure the heap property
     */
    private void percolateUp(int i) {
        int parent = (i - 1) / 2;

        if (parent >= 0 && heap.get(parent) < heap.get(i)) {
            Collections.swap(heap, parent, i);
            percolateUp(parent);
        }
    }
}
```

**Output**
```
h.peek() = 10
h.size() = 6
10 3 7 2 1 5 
h.delete() = 10
h.peek() = 7
h.size() = 5
7 3 5 2 1 
```

### **Complexity Analysis**
| Operation  | Time Complexity                              | Space Complexity |
| ---------- | -------------------------------------------- | ---------------- |
| **Insert** | **O(log n) (or O(n) in your code)**          | O(1)             |
| **Delete** | **O(log n) (or O(n) in your code)**          | O(1)             |
| **Peek**   | O(1)                                         | O(1)             |
| **Size**   | O(1)                                         | O(1)             |
| **Print**  | O(n)                                         | O(1)             |
