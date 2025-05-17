# Implementation Doubly LinkedList 

```java
class ListNode {
    int val;
    ListNode prev;
    ListNode next;

    public ListNode(int val) {
        this.val = val;
        prev = null;
        next = null;
    }
}


class DoublyLinkedList {
    ListNode head = null; // initially assign head to null
    int len = 0; // initially doubly linked list is empty, so length is 0

    public void displayList() {
        // currentNode is use to traverse through the list to print the elements
        // we don't want to loose the access of head
        ListNode cur = head;
        while (cur != null) {
            System.out.print(cur.val + " ");
            cur = cur.next;
        }
        System.out.println();
    }

    public void addFirst(int val) {
        ListNode cur = head;
        ListNode newNode = new ListNode(val);

        newNode.next = cur; // make new node point to current head
        if (cur != null) cur.prev = newNode; // make current head point to its previous node
        head = newNode; // update head to new node
        len++; // increment the length
    }

    public void addLast(int val) {
        ListNode cur = head;
        ListNode newNode = new ListNode(val);

        // moving the current node to the last element
        while (cur.next != null) {
            cur = cur.next;
        }

        cur.next = newNode; // make the current node point to newNode, which is last node.
        newNode.prev = cur; // make newNode point to current node
        len++;
    }

    public void addAt(int val, int index) { // 0 based indexing
        if(index < 0 || index > len) return; // index out of bounds
        if(index == 0){
            deleteFirst();
        } else if(index == len){
            deleteLast();
        } else {
            ListNode newNode = new ListNode(val); // Create a new node
            ListNode cur = head; // current node.

            /**
             * 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> null
             * 0    1    2    3    4    5
             * index = 3, addNode = 100
             * 1 -> 2 -> 3 -> 100 -> 4 -> 5 -> 6 -> null
             */

            // Moving the current node to the index-1-1th element
            for (int i = 0; i < index - 1; i++) {
                cur = cur.next;
            }

            newNode.next = cur.next; // make the newNode point to the current's next node
            cur.next.prev = newNode; // make the current after node point to the newNode

            cur.next = newNode; // make the current node point to the newNode
            newNode.prev = cur; // make the newNode point to the current node
            len++; // Increase teh length
        }
    }

    /**
     * Deletes the first element of the doubly linked list.
     * time: O(1)
     */
    public void deleteFirst() {
        if (head == null) return;
        if (head.next == null) { // single element in the list
            head = null; // set head to null
        } else {
            // 1 -> 2 -> 3 -> 4 -> 5 -> null
            head = head.next; // move head to next node
            head.prev = null; // set head's previous point to null
        }
        len--; // decrease length
    }

    /**
     * Deletes the last element of the doubly linked list.
     * time: O(n)
     */
    public void deleteLast() {
        if (head == null) return;
        if (head.next == null) { // single element
            head = null; // set head to null
        } else {
            // Move to the last element
            ListNode tail = head;
            while (tail.next != null) {
                tail = tail.next;
            }

            /**
             * 1 -> 2 -> 3 -> 4 -> 5 -> null
             * 0    1    2    3    4
             * last element: 5
             * Making 4th next to null and 5th Prev to null
             * whatever the element presents before the last element, its next should point to null.
             */
            tail.prev.next = null;
            tail.prev = null; // set last previous to null
        }
        len--;
    }

    /**
     * Deletes an element at any specific index of the doubly linked list.
     * time: O(n)
     */
    public void deleteAt(int index) {
        if (index < 0 || index >= len) return;
        if(index == 0){
            deleteFirst();
        } else if (index == len - 1) {
            deleteLast();
        } else {
            /**
             * 1 -> 2 -> 3 -> 4 -> 5 -> null
             * 0    1    2    3    4
             * deleteNode = 4, at Index = 3
             *
             * 1 -> 2 -> 3 -> 5 -> null
             */
            ListNode cur = head;
            for (int i = 0; i < index ; i++) {
                cur = cur.next;
            }
            ListNode delNodeFront = cur.next; // front node of the nod we want to delete
            ListNode delNodeBack = cur.prev; // back node of the node we want to delete

            delNodeBack.next = delNodeFront;
            delNodeFront.prev = delNodeBack;
        }
        len --;
    }

    public int length() {
        return len;
    }
}

// Main class
public class Main {
    public static void main(String[] args) {
        DoublyLinkedList dl = new DoublyLinkedList();

        System.out.println("Adding elements at first: ");
        dl.addFirst(10);
        dl.addFirst(20);
        dl.addFirst(30);
        dl.displayList();

        System.out.println("Adding elements at last: ");
        dl.addLast(40);
        dl.addLast(50);
        dl.displayList();

        System.out.println("Adding elements 100 at index: 4");
        dl.addAt(100, 4);
        dl.displayList();

        System.out.println("Delete first element or head");
        dl.deleteFirst();
        dl.displayList();

        System.out.println("Delete last element or tail");
        dl.deleteLast();
        dl.displayList();

        System.out.println("Delete at index 2");
        dl.deleteAt(2);
        dl.displayList();

        System.out.println("current length: "+dl.length());
    }
}
```

**Output**
```
Adding elements at first: 
30 20 10 
Adding elements at last: 
30 20 10 40 50 
Adding elements 100 at index: 4
30 20 10 40 100 50 
Delete first element or head
20 10 40 100 50 
Delete last element or tail
20 10 40 100 
Delete at index 2
20 10 100 
current length: 3
```