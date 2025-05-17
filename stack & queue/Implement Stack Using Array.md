# Implement Stack Using Array

| Difficulty | Topic           | Companies | Resources   |
| ---------- | --------------- | --------- | ----------- |
| **Easy**   | Stack           |           | [Video](https://youtu.be/tqQ5fTamIN4?si=i_8LPsLOovllcoyn)   |
|            |                 |           | [Article]() |

### Code
```java
/**
 * Here, We Implement the Stack data structure using an Array
 * Implement all the important methods of Stack like, push, pop, peek, size.
 * Stack follows the LIFO (Last in First out) principle.
 */
class Stack {
    int[] arr = null; // empty array
    int top;

    /**
     * Initialize the stack with the given size
     * @param size size of the stack
     */
    public Stack(int size){
        // Since, an array is not dynamic. so, It Sizes should be predefined
        arr = new int[size];
        top = -1; // track the element that added last or removed first
    }


    /**
     * Check if the stack is full or not
     * @return true if stack is full, false otherwise
     */
    public boolean isFull(){
        return (top == (arr.length-1));
    }

    /**
     * Check if the stack is empty or not
     * @return true if stack is empty, false otherwise
     */
    public boolean isEmpty(){
        return (top == -1);
    }

    /**
     * push
     * Add the element to the top of the stack
     * @param data element to be added
     */
    public void push(int data){
        if(isFull()){
            System.out.println("stack is full");
        } else {
            top ++; // first increment the top
            arr[top] = data; // assign value to the top
        }
    }

    /**
     * This method is used to remove the top element from the stack.
     * @return the top element of the stack
     */
    public int pop() {
        if (isEmpty()) {
            return -1; // Return -1 if the stack is empty
        } else {
            int topValue = arr[top]; // Store the top value to return later
            top--; // Decrement the top index
            return topValue; // Return the stored top value
        }
    }


    /**
     * This method is used to get the top element of the stack.
     * @return the top element of the stack
     */
    public int peek(){
        if(isEmpty()) return -1;
        else{
            return arr[top];
        }
    }

    /**
     * This method is used to get the current size of the stack.
     * It simply returns the top pointer + 1
     * @return the size of the stack
     */
    public int size(){
        return top+1;
    }

    /**
     * This method is used to display all the elements of the stack.
     * It doesn't follow the LIFO principle, it just prints all the elements from the starting to the top.
     */
    public void display(){
        if(isEmpty()) System.out.println("Stack is Empty!");
        else{
            for(int i = 0; i <= top; i ++){
                System.out.print(arr[i]+" ");
            }
            System.out.println();
        }
    }
}
public class StackUsingArray {
    public static void main(String[] args) {
        Stack st = new Stack(5);

        st.push(10);
        st.push(20);
        st.push(30);
        System.out.println(st.pop());
        st.push(40);
        st.push(50);
        System.out.println(st.pop());
        System.out.println(st.peek());

        System.out.println(st.size());
        System.out.println(st.isEmpty());
        System.out.println(st.isFull());

        System.out.println("Display elements: ");
        st.display();
    }
}
```
**Output**
```
30
50
40
3
false
false
Display elements: 
10 20 40 
```

### Complexity Analysis
Here's a table showing the **time and space complexity** of various **stack operations** in Java, particularly when using `java.util.Stack` or `Deque` (like `ArrayDeque`) as a stack:

| **Operation**      | **Description**                               | **Time Complexity** | **Space Complexity** |
| ------------------ | --------------------------------------------- | ------------------- | -------------------- |
| `push(element)`    | Add element to top of stack                   | O(1)                | O(1)                 |
| `pop()`            | Remove and return top element                 | O(1)                | O(1)                 |
| `peek()` / `top()` | Return top element without removing           | O(1)                | O(1)                 |
| `isEmpty()`        | Check if stack is empty                       | O(1)                | O(1)                 |
| `size()`           | Return number of elements in the stack        | O(1)                | O(1)                 |
| `search(element)`  | Return position of element from top (1-based) | O(n)                | O(1)                 |
| **Initialization** | Creating a new empty stack                    | O(1)                | O(1)                 |

> ✅ Most stack operations in Java (`push`, `pop`, `peek`, etc.) are **constant time** (O(1)) due to the internal structure (like using an array or linked list under the hood).  
> ❗ However, `search()` is **linear** because it may need to traverse the entire stack.
