# Implementation of Singly LinkedList

```java
// ListNode for LinkedList
class ListNode {
    // value/data and next pointer of the linked list
    int val; 
    ListNode next; 

    public ListNode(int val) {
        this.val = val;
    }
    public ListNode(int val, ListNode next){
        this.val = val;
        this.next = next;
    }
}

/**
 * This class implements methods for linked list operations.
 * add- addFirst(), addLast(), addAt() 
 * delete - deleteFirst(), deleteLast(), deleteAt()
 * length()
 * display()
 */
public class SinglyLinkedList {
    // initialize the length, which is empty for now
    // also, initialize the head pointing to the null
    private int len = 0; 
    private ListNode head = null;

    /**
     * It will add an element at the front of the linked list.
     */
    public void addFirst(int val) {
        ListNode newListNode = new ListNode(val);
        
        // make new node point to current head
        newListNode.next = head; 

        // update head to new node
        head = newListNode; 
        len++;
    }

    /**
     * It will add an element at the last position of the linked list.
     */
    public void addLast(int val) {
        ListNode newListNode = new ListNode(val);
        // if list is empty, set head to new node
        if (head == null) { 
            head = newListNode;
        } else {
            ListNode cur = head;

            // reach to last node
            while (cur.next != null) { 
                cur = cur.next;
            }

            // link last node to new node
            cur.next = newListNode; 
        }
        len++;
    }

    /**
     * Adds an element at a specified index in the linked list.
     */
    public void addAt(int val, int index) {
        // index out of bounds
        if (index < 0 || index > len) return; 
        
        // handle adding at head
        if (index == 0) {
            addFirst(val);
            return;
        }

        // handle adding at end
        if (index == len) {
            addLast(val); 
            return;
        }

        ListNode newListNode = new ListNode(val);
        ListNode cur = head;

        // reach (index-1)th node
        for (int i = 0; i < index - 1; i++) { 
            cur = cur.next;
        }

        // link new node to current's next node
        newListNode.next = cur.next; 
        
        // link current node to new node
        cur.next = newListNode; 
        len++;
    }

    /**
     * Deletes the first element of the linked list.
     */
    public void deleteFirst() {
        // check if list is not empty
        if (head != null) { 
            // update head to the next node
            head = head.next; 
            len--;
        }
    }

    /**
     * Deletes the last element of the linked list.
     */
    public void deleteLast() {
        // check if list is empty
        if (head == null) return; 

        // only one element in list
        if (head.next == null) { 
            head = null;
        } else {
            ListNode cur = head;

            // reach second last node
            while (cur.next.next != null) { 
                cur = cur.next;
            }

            // remove last node by setting next of second last to null
            cur.next = null; 
        }

        len--;
    }

    /**
     * Deletes an element at a specified index in the linked list.
     */
    public void deleteAt(int index) {
        // index out of bounds
        if (index < 0 || index >= len) return; 

        // handle deleting from head
        if (index == 0) {
            deleteFirst(); 
            return;
        }

        // handle deleting from end
        if (index == len - 1) {
            deleteLast(); 
            return;
        }

        ListNode cur = head;

        // reach (index-1)th node
        for (int i = 0; i < index - 1; i++) { 
            cur = cur.next;
        }

        // bypass deleted node
        cur.next = cur.next.next; 
        len--;
    }

    /**
     * Displays all elements in the linked list.
     */
    public void displayList() {
        if (head == null) {
            System.out.println("The list is empty.");
            return;
        }
        ListNode cur = head;

        while (cur != null) {
            System.out.print(cur.val + " ");
            cur = cur.next;
        }

        System.out.println();
    }

    /**
     * Returns the number of elements in the linked list.
     */
    public int length() {
        return len;
    }
    /**
     * public int getLength(Node head) {
     *         int len = 0;
     *         Node cur = head;
     *         while(cur!= null){
     *             len ++;
     *             cur = cur.next;
     *         }
     *         return len;
     *     }
     */
}


/**
 * Main class to demonstrate linked list operations.
 */
public class Main {
    public static void main(String[] args) {
        SinglyLinkedList list = new SinglyLinkedList();

        // Add elements at the front
        list.addFirst(10);
        list.addFirst(20);
        list.addFirst(30);
        System.out.print("After adding first: ");
        list.displayList();

        // Add elements at the last
        list.addLast(40);
        list.addLast(60);
        System.out.print("After adding last: ");
        list.displayList();

        // Add element at a specific index
        list.addAt(100, 4);
        System.out.print("After adding at index 4: ");
        list.displayList();

        // Delete first element
        list.deleteFirst();
        System.out.print("After deleting first: ");
        list.displayList();

        // Delete last element
        list.deleteLast();
        System.out.print("After deleting last: ");
        list.displayList();

        // Delete element at a specific index
        list.deleteAt(2);
        System.out.print("After deleting at index 2: ");
        list.displayList();

        System.out.println("Length of the linked list: " + list.length());
    }
}
```

**Output**
```
After adding first: 30 20 10 
After adding last: 30 20 10 40 60 
After adding at index 4: 30 20 10 40 100 60 
After deleting first: 20 10 40 100 60 
After deleting last: 20 10 40 100 
After deleting at index 2: 20 10 100 
Length of the linked list: 3
```