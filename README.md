public class ArrayList implements List {
    // Define the List interface inside the same file
    public interface List {
        public void createList(int n);
        public void insertFirst(Object ob);
        public void insertAfter(Object item, Object pos);
        public Object deleteFirst();
        public Object deleteAfter(Object pos);
        public boolean isEmpty();
        public int size();
    }

    static class Node { // Make Node static to avoid holding a reference to ArrayList
        Object data;
        int next;

        Node(Object ob, int i) {
            data = ob;
            next = i;
        }
    }

    int MAXSIZE; // max number of nodes in the list
    Node list[]; // create list array
    int head, count; // count: current number of nodes in the list

    public ArrayList(int s) { // Constructor
        MAXSIZE = s;
        list = new Node[MAXSIZE];
        head = -1;
        count = 0;
    }

    public void initializeList() { // Initialize the list with empty nodes
        for (int p = 0; p < MAXSIZE - 1; p++)
            list[p] = new Node(null, p + 1);
        list[MAXSIZE - 1] = new Node(null, -1);
    }

    public void createList(int n) { // Create ‘n’ nodes
        if (n > MAXSIZE) {
            System.out.println("***Cannot create more nodes than MAXSIZE");
            return;
        }

        int p;
        for (p = 0; p < n; p++) {
            list[p] = new Node(11 + 11 * p, p + 1);
            count++;
        }
        list[p - 1].next = -1; // end of the list
        head = 0; // set head to the first element
    }

    public void insertFirst(Object item) {
        if (count == MAXSIZE) {
            System.out.println("***List is FULL");
            return;
        }
        int p = getNode();
        if (p != -1) {
            list[p].data = item;
            if (isEmpty())
                list[p].next = -1;
            else
                list[p].next = head;
            head = p;
            count++;
        }
    }

    public void insertAfter(Object item, Object x) {
        if (count == MAXSIZE) {
            System.out.println("***List is FULL");
            return;
        }
        int q = getNode(); // get the available position to insert new node
        int p = find(x); // get the index (position) of the Object x

        if (p == -1) {
            System.out.println("***Element " + x + " not found in the list");
            return;
        }

        if (q != -1) {
            list[q].data = item;
            list[q].next = list[p].next;
            list[p].next = q;
            count++;
        }
    }

    public int getNode() { // returns available node index
        for (int p = 0; p < MAXSIZE; p++)
            if (list[p] != null && list[p].data == null) return p;
        return -1;
    }

    public int find(Object ob) { // find the index (position) of the Object ob
        int p = head;
        while (p != -1) {
            if (list[p] != null && list[p].data.equals(ob)) return p;
            p = list[p].next; // advance to next node
        }
        return -1;
    }

    public Object deleteFirst() {
        if (isEmpty()) {
            System.out.println("List is empty: no deletion");
            return null;
        }
        Object tmp = list[head].data;
        if (list[head].next == -1) // if the list contains one node,
            head = -1; // make list empty.
        else
            head = list[head].next;
        count--; // update count
        return tmp;
    }

    public Object deleteAfter(Object x) {
        int p = find(x);
        if (p == -1 || list[p].next == -1) {
            System.out.println("No deletion");
            return null;
        }
        int q = list[p].next;
        Object tmp = list[q].data;
        list[p].next = list[q].next;
        count--;
        return tmp;
    }

    public void display() {
        int p = head;
        if (isEmpty()) {
            System.out.println("\nList is empty.");
            return;
        }

        System.out.print("\nList: [ ");
        while (p != -1) {
            System.out.print(list[p].data + " "); // print data
            p = list[p].next; // advance to next node
        }
        System.out.println("]\n");
    }

    public boolean isEmpty() {
        return count == 0;
    }

    public int size() {
        return count;
    }

    public static void main(String[] args) {
        ArrayList linkedList = new ArrayList(10);
        linkedList.initializeList();
        linkedList.createList(4);
        linkedList.display();

        System.out.print("InsertFirst 55: ");
        linkedList.insertFirst(55);
        linkedList.display();

        System.out.print("Insert 66 after 33: ");
        linkedList.insertAfter(66, 33); // insert 66 after 33
        linkedList.display();

        Object item = linkedList.deleteFirst();
        System.out.println("Deleted node: " + item);
        linkedList.display();

        System.out.println("InsertFirst 77: ");
        linkedList.insertFirst(77);
        linkedList.display();

        item = linkedList.deleteAfter(22); // delete node after node 22
        System.out.println("Deleted node: " + item);
        linkedList.display();

        System.out.println("size(): " + linkedList.size());
    }
}
