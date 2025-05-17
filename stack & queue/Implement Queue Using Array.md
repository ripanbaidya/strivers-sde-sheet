# Implement Queue Using Array

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Queue        |                     | [Video](https://youtu.be/tqQ5fTamIN4?si=i_8LPsLOovllcoyn)   |
|            |              |                     | [Article]() |

### Code
```java
/**
 * Queue implementation using an array.<br>
 *
 * Queue is a data structure which follows First-In-First-Out (FIFO) order. It
 * has two main operations: <br>
 * 1. Enqueue (push): adds an element to the end of the queue.<br>
 * 2. Dequeue (pop): removes an element from the front of the queue.<br>
 *
 * Additionally, it has two auxiliary operations: <br>
 * 1. Peek: returns the element at the front of the queue without removing it.<br>
 * 2. Size: returns the number of elements in the queue.<br>
 *
 * Pointers:<br>
 * Front: points to the front of the queue (deletion happens here).<br>
 * Rear: points to the end of the queue (insertion happens here).<br>
 * */
class Queue {
    int[] arr;
    int length, currSize, front, rear;

    // Constructor to initialize the queue with a given length
    public Queue(int length) {
        this.length = length;
        arr = new int[length];
        front = rear = -1;
        currSize = 0;
    }

    // Method to add an element to the queue
    public void push(int data) {
        if (currSize == length) {
            System.out.println("Queue is Full");
            System.exit(1);
        }
        // If the queue is empty, set front and rear to 0
        if (currSize == 0) {
            front = rear = 0;
        } else {
            rear = (rear + 1) % length; // Circular increment for rear
        }
        arr[rear] = data; // Insert the data at the rear
        System.out.println("Element " + data + " Inserted Successfully.");
        currSize++;
    }

    // Method to remove and return the front element of the queue
    public int pop() {
        if (currSize == 0) return -1; // queue is empty

        int poopedValue = arr[front]; // Store the front value
        if (currSize == 1) { // If it was the last element, reset front and rear
            front = rear = -1;
        } else {
            front = (front + 1) % length; // Circular increment for front
        }
        currSize--;
        return poopedValue;
    }

    // Method to return the front element without removing it
    public int peek() {
        if (currSize == 0)  return -1; // queue is empty
        return arr[front];
    }

    // Method to return the current size of the queue
    public int size() {
        return currSize;
    }
}
public class QueueUsingArray {
    public static void main(String[] args) {
        Queue q = new Queue(5);

        q.push(10);
        q.push(20);
        q.push(30);

        System.out.println(q.peek());
        System.out.println(q.pop());
        System.out.println(q.pop());
    }
}
```
**Output**
```
Element 10 Inserted Successfully.
Element 20 Inserted Successfully.
Element 30 Inserted Successfully.
peek value: 10
pop value: 10
pop value: 20
size : 1
```

Here's a breakdown of the **time and space complexity** for each operation in your `QueueUsingArray` implementation, shown in a clear table format:


### ✅ **Time and Space Complexity**

| **Operation**    | **Description**                                   | **Time Complexity** | **Space Complexity** |
| ---------------- | ------------------------------------------------- | ------------------- | -------------------- |
| `push(int data)` | Inserts an element at the rear (circularly)       | O(1)                | O(1)                 |
| `pop()`          | Removes and returns element from front (circular) | O(1)                | O(1)                 |
| `peek()`         | Returns the front element without removing        | O(1)                | O(1)                 |
| `size()`         | Returns the number of elements in the queue       | O(1)                | O(1)                 |
| **Constructor**  | Initializes array and variables                   | O(n) (for `arr`)    | O(n)                 |

> Where **`n`** is the capacity of the queue passed to the constructor.


### ⚙️ Notes:
- I’ve implemented a **circular queue using an array**, so **all queue operations are constant time** (very efficient).
- Space complexity for each method is `O(1)` because operations are done in-place.  
- The only **O(n)** space/time cost is at initialization, where the internal array of size `n` is allocated.
