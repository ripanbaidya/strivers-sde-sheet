# [Implement Stack Using Queue](https://leetcode.com/problems/implement-stack-using-queues/description/)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Stack        |                     | [Video](https://youtu.be/tqQ5fTamIN4?si=i_8LPsLOovllcoyn)   |
|            | Queue        |                     | [Article]() |

## ✅ Stack using Two Queues

### Code
```java
import java.util.LinkedList;
import java.util.Queue;

class Stack {
    Queue<Integer> q1 = new LinkedList<>();
    Queue<Integer> q2 = new LinkedList<>();

    // Push element onto stack
    public void push(int data) {
        q2.add(data); // Step 1: enqueue new element to q2

        // Step 2: move all elements from q1 to q2
        while (!q1.isEmpty()) {
            q2.add(q1.poll());
        }

        // Step 3: swap q1 and q2 references
        Queue<Integer> temp = q1;
        q1 = q2;
        q2 = temp;
    }

    // Removes and returns the top element
    public int pop() {
        return q1.isEmpty() ? -1 : q1.poll();
    }

    // Returns the top element without removing
    public int peek() {
        return q1.isEmpty() ? -1 : q1.peek();
    }

    // Returns current size of the stack
    public int size() {
        return q1.size();
    }

    // Displays the stack (top to bottom)
    public void display() {
        for (int el : q1) {
            System.out.print(el + " ");
        }
        System.out.println();
    }
}
/**
 * Stack implementation using two queues (q1 is main, q2 is helper)
 *
 * push() - O(n) | pop(), peek(), size() - O(1)
 */
public class Main {
    public static void main(String[] args) {
        Stack stack = new Stack();

        stack.push(10);
        stack.push(20);
        stack.push(30);

        stack.display();           // Output: 30 20 10
        System.out.println(stack.peek()); // Output: 30
        System.out.println(stack.pop());  // Output: 30

        stack.push(40);
        stack.display();           // Output: 40 20 10

        System.out.println(stack.pop());  // Output: 40
        System.out.println(stack.peek()); // Output: 20
    }
}
```
**Output** 
```
30 20 10 
30
30
40 20 10 
40
20
```

### 📊 Time and Space Complexity

| **Operation**    | **Description**                         | **Time Complexity** | **Space Complexity**  |
| ---------------- | --------------------------------------- | ------------------- | --------------------- |
| `push(int data)` | Adds element to stack using two queues  | O(n)                | O(n) (due to copying) |
| `pop()`          | Removes and returns top element         | O(1)                | O(1)                  |
| `peek()`         | Returns top element without removing    | O(1)                | O(1)                  |
| `size()`         | Returns number of elements in the stack | O(1)                | O(1)                  |
| `display()`      | Prints all elements from top to bottom  | O(n)                | O(1)                  |
| **Constructor**  | Initializes two empty queues            | O(1)                | O(1)                  |



## ✅ Stack using One Queue
```java
import java.util.LinkedList;
import java.util.Queue;

/**
 * Stack implementation using a single queue.
 * After each push, rotate the queue so the new element is at the front.
 * 
 * push - O(n), pop/peek/size - O(1)
 */
class Stack {
    Queue<Integer> q1 = new LinkedList<>();

    // Push element onto the stack
    public void push(int data) {
        q1.add(data); // Enqueue new element

        // Rotate the queue to place new element at front
        for (int i = 1; i <= q1.size()-1; i++) {
            q1.add(q1.poll());
        }
    }

    // Remove and return top element
    public int pop() {
        return q1.isEmpty() ? -1 : q1.poll();
    }

    // Return top element without removing
    public int peek() {
        return q1.isEmpty() ? -1 : q1.peek();
    }

    // Return current size of the stack
    public int size() {
        return q1.size();
    }

    // Display stack from top to bottom
    public void display() {
        q1.forEach(el -> System.out.print(el + " "));
        System.out.println();
    }
}
public class Main {
    public static void main(String[] args) {
        Stack stack = new Stack();

        stack.push(10);
        stack.push(20);
        stack.push(30);

        stack.display();               // Output: 30 20 10
        System.out.println(stack.peek()); // Output: 30
        System.out.println(stack.pop());  // Output: 30

        stack.push(40);

        stack.display();               // Output: 40 20 10
        System.out.println(stack.pop());  // Output: 40
        System.out.println(stack.peek()); // Output: 20
    }
}
```
**Output**
```
30 20 10 
30
30
40 20 10 
40
20
```
### 📊 Time and Space Complexity Table

| **Operation**    | **Description**                                  | **Time Complexity** | **Space Complexity** |
| ---------------- | ------------------------------------------------ | ------------------- | -------------------- |
| `push(int data)` | Adds element and rotates queue to simulate stack | O(n)                | O(1)                 |
| `pop()`          | Removes and returns top element                  | O(1)                | O(1)                 |
| `peek()`         | Returns top element without removing             | O(1)                | O(1)                 |
| `size()`         | Returns number of elements in the stack          | O(1)                | O(1)                 |
| `display()`      | Displays all elements (top to bottom)            | O(n)                | O(1)                 |
| **Constructor**  | Initializes empty queue                          | O(1)                | O(1)                 |

> 🧠 In this version, **`push()` is expensive** because it reorders the queue. But the benefit is that **`pop()` and `peek()` become O(1)**.
