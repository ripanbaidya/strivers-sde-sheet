# [Implement Queue Using Stack](https://leetcode.com/problems/implement-queue-using-stacks/description/)

| Difficulty | Topic        | Companies           | Resources   |
| ---------- | ------------ | ------------------- | ----------- |
| Medium     | Queue        |                     | [Video](https://youtu.be/tqQ5fTamIN4?si=i_8LPsLOovllcoyn)   |
|            | Stack        |                     | [Article]() |


## ✅ Queue Using Two Stacks (`push()` Costly)

### Code
```java
class MyQueue {
    Stack<Integer> input = new Stack<>();
    Stack<Integer> output = new Stack<>();

    public MyQueue() {}

    // push 
    public void push(int x) {
        // step 1. Move all elements from input to output stack
        while (!input.isEmpty()) {
            output.push(input.pop());
        }

        // Push new element to input
        System.out.println("The element pushed is " + x);
        input.push(x);

        // Move all elements from output to input stack
        while (!output.isEmpty()) {
            input.push(output.pop());
        }
    }

    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if (input.isEmpty()) {
            return -1;
        }
        return input.pop();
    }

    /** Get the front element. */
    public int peek() {
        if (input.isEmpty()) {
            return -1;
        }
        return input.peek();
    }

    /** Returns the size of the queue. */
    public int size() {
        return input.size();
    }
}

class TUF {
    public static void main(String[] args) {
        MyQueue q = new MyQueue();
        q.push(3);
        q.push(4);
        System.out.println("The element popped is " + q.pop());
        q.push(5);
        System.out.println("The top element is " + q.peek());
        System.out.println("The size of the queue is " + q.size());
    }
}
```
**Output**
```
The element pushed is 3
The element pushed is 4
The element popped is 3
The element pushed is 5
The top element is 4
The size of the queue is 2
```


### 📊 Time and Space Complexity

| **Operation**     | **Description**                                  | **Time Complexity** | **Space Complexity** |
| ----------------- | ------------------------------------------------ | ------------------- | -------------------- |
| `push(x)`         | Transfers all elements to output and back again  | O(n)                | O(n)                 |
| `pop()`           | Removes the front (top of input)                 | O(1)                | O(1)                 |
| `peek()`          | Returns the front element                        | O(1)                | O(1)                 |
| `size()`          | Returns the size of the input stack (i.e. queue) | O(1)                | O(1)                 |
| **Overall Space** | Uses two stacks                                  | —                   | O(2n)                |

> `n` = number of elements in the queue  
> Space is `O(2n)` because at peak, both stacks could store `n` elements during a transfer.



## ✅ Queue Using Two Stacks (`amortized cost O(1) for pop() and peek()`)

### Code
```java
class MyQueue {
    Stack<Integer> input = new Stack<>();
    Stack<Integer> output = new Stack<>();

    // Constructor
    public MyQueue() {}

    // Push element to the back of the queue
    public void push(int x) {
        System.out.println("The element pushed is " + x);
        input.push(x);
    }

    // Removes and returns the front element
    public int pop() {
        shiftStacksIfNeeded();
        if (output.isEmpty()) {
            System.out.println("Queue is empty");
            return -1;
        }
        return output.pop();
    }

    // Returns the front element
    public int peek() {
        shiftStacksIfNeeded();
        if (output.isEmpty()) {
            System.out.println("Queue is empty");
            return -1;
        }
        return output.peek();
    }

    // Returns current size of the queue
    public int size() {
        return input.size() + output.size();
    }

    // Helper method to transfer input to output if output is empty
    private void shiftStacksIfNeeded() {
        if (output.isEmpty()) {
            while (!input.isEmpty()) {
                output.push(input.pop());
            }
        }
    }
}

class Main {
    public static void main(String[] args) {
        MyQueue q = new MyQueue();

        q.push(3);
        q.push(4);
        System.out.println("The element popped is " + q.pop());
        q.push(5);
        System.out.println("The top element is " + q.peek());
        System.out.println("The size of the queue is " + q.size());
    }
}
```

**Output**
```
The element pushed is 3
The element pushed is 4
The element popped is 3
The element pushed is 5
The top element is 4
The size of the queue is 2
```

### 📊 Time and Space Complexity Table

| **Operation**     | **Description**                      | **Time Complexity** | **Space Complexity** |
| ----------------- | ------------------------------------ | ------------------- | -------------------- |
| `push(x)`         | Adds element to `input` stack        | O(1)                | O(1)                 |
| `pop()`           | Transfers to `output` only if needed | Amortized O(1)      | O(1) per operation   |
| `peek()`          | Transfers to `output` only if needed | Amortized O(1)      | O(1) per operation   |
| `size()`          | Returns size of both stacks          | O(1)                | O(1)                 |
| **Overall Space** | Uses two stacks                      | —                   | O(2n)                |

> 🔁 The amortized cost of `pop()` and `peek()` is **O(1)** because each element is moved between stacks **only once**, even if queried many times later.

