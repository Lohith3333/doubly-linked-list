# doubly-linked-list
class DL {
    Node head;
    Node tail;
    int size;

    public DL() {
        this.size = 0;
    }

    // Insert at the beginning
    public void insertFirst(int val) {
        Node node = new Node(val);
        node.next = head;
        if (head != null) {
            head.prev = node;
        }
        head = node;

        if (tail == null) {
            tail = head;
        }
        size++;
    }

    // Insert at the end
    public void insertLast(int val) {
        if (tail == null) {
            insertFirst(val);
            return;
        }
        Node node = new Node(val);
        tail.next = node;
        node.prev = tail;
        tail = node;
        size++;
    }

    // Insert at a specific position
    public void insertPos(int index, int val) {
        if (index == 0) {
            insertFirst(val);
            return;
        }
        if (index == size) {
            insertLast(val);
            return;
        }

        Node temp = get(index - 1);
        Node node = new Node(val);
        node.next = temp.next;
        node.prev = temp;

        temp.next.prev = node;
        temp.next = node;
        size++;
    }

    // Delete first node
    public int deleteFirst() {
        if (head == null) {
            throw new RuntimeException("List is empty");
        }

        int val = head.value;
        head = head.next;
        if (head != null) {
            head.prev = null;
        } else {
            tail = null;
        }
        size--;
        return val;
    }

    // Delete last node
    public int deleteLast() {
        if (size <= 1) {
            return deleteFirst();
        }
        int val = tail.value;
        tail = tail.prev;
        tail.next = null;
        size--;
        return val;
    }

    // Delete node at index
    public int delete(int index) {
        if (index == 0) {
            return deleteFirst();
        }
        if (index == size - 1) {
            return deleteLast();
        }

        Node temp = get(index);
        int val = temp.value;
        temp.prev.next = temp.next;
        temp.next.prev = temp.prev;
        size--;
        return val;
    }

    // Find node by value
    public Node find(int val) {
        Node temp = head;
        while (temp != null) {
            if (temp.value == val) {
                return temp;
            }
            temp = temp.next;
        }
        return null;
    }

    // Get node at index
    public Node get(int index) {
        Node temp = head;
        for (int i = 0; i < index; i++) {
            temp = temp.next;
        }
        return temp;
    }

    // Display forward
    public void display() {
        Node temp = head;
        while (temp != null) {
            System.out.print(temp.value + " -> ");
            temp = temp.next;
        }
        System.out.println("end");
    }

    // Display backward
    public void displayReverse() {
        Node temp = tail;
        while (temp != null) {
            System.out.print(temp.value + " <- ");
            temp = temp.prev;
        }
        System.out.println("start");
    }

    // Node class
    class Node {
        int value;
        Node next;
        Node prev;

        public Node(int value) {
            this.value = value;
        }
    }
}
public class Main {
    public static void main(String[] args) {
        DL list = new DL();
        list.insertFirst(3);
        list.insertFirst(2);
        list.insertFirst(1);
        list.display();          // 1 -> 2 -> 3 -> end
        list.displayReverse();   // 3 <- 2 <- 1 <- start

        list.insertLast(4);
        list.insertLast(5);
        list.display();          // 1 -> 2 -> 3 -> 4 -> 5 -> end

        list.insertPos(3, 99);
        list.display();          // 1 -> 2 -> 3 -> 99 -> 4 -> 5 -> end

        System.out.println("Deleted first: " + list.deleteFirst());
        System.out.println("Deleted last: " + list.deleteLast());
        System.out.println("Deleted at index 2: " + list.delete(2));
        list.display();

        DL.Node found = list.find(99);
        if (found != null) {
            System.out.println("Found node with value: " + found.value);
        } else {
            System.out.println("Value not found");
        }

        list.displayReverse();  // Display the list in reverse
    }
}

