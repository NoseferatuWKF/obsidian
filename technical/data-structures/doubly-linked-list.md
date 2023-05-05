
```java
/*
it should look something like this:
	[A]<->[B]<->[C]
adding or removing is constant time, but the traversal is O(n) generally
*/
interface IDoublyLinkedList<T> {
    public int length();

    public void insertAt(T data, int index);

    public T remove(T data);

    public T removeAt(int index);

    public void append(T data);

    public void prepend(T data);

    public T get(int index);
}

public class DoublyLinkedList<T> implements IDoublyLinkedList<T> {
    Node<T> head;
    Node<T> tail;
    int length;

    class Node<K> {
        K data;
        Node<K> prev;
        Node<K> next;

        public Node(K data) {
            this.data = data;
            this.prev = this.next = null;
        }
    }

    public DoublyLinkedList() {
        this.head = this.tail = null;
        this.length = 0;
    }

    @Override
    public int length() {
        return this.length;
    }

    @Override
    public void insertAt(T data, int index) {
        Node<T> node = new Node<T>(data);
        Node<T> curr = this.traversal(index);
        Node<T> prev = curr.prev;
        prev.next = node;
        node.next = curr;
        node.prev = prev;
        curr.prev = node;

        this.length++;
    }

    @Override
    public T remove(T data) {
        if (this.length == 0) {
            return null;
        }
        Node<T> curr = this.head;
        // because of autoboxing we need to do this shit
        while (!curr.data.equals(data) && curr != null) {
            curr = curr.next;
        }
        if (curr == null) {
            return null;
        }
        curr.prev.next = curr.next;
        curr.prev = curr.next = null;

        this.length--;
        return curr.data;
    }

    @Override
    public T removeAt(int index) {
        if (this.length == 0 || index > this.length) {
            return null;
        }
        Node<T> curr = this.traversal(index);
        curr.prev.next = curr.next;
        curr.next.prev = curr.prev;
        curr.prev = curr.next = null;
        
        this.length--;
        return curr.data;
    }

    @Override
    public void append(T data) {
        Node<T> node = new Node<T>(data);
        if (this.length == 0) {
            this.head = this.tail = node;
        }
        node.prev = this.tail;
        this.tail.next = this.tail = node;

        this.length++;
    }

    @Override
    public void prepend(T data) {
        Node<T> node = new Node<T>(data);
        if (this.length == 0) {
            this.head = this.tail = node;
        }
        node.next = this.head;
        this.head.prev = this.head = node;

        this.length++;
    }

    @Override
    public T get(int index) {
        if (index > this.length) {
            return null;
        }
        Node<T> node = this.traversal(index);
        return node.data;
    }

    Node<T> traversal(int index) {
        int i = 0;
        Node<T> curr = this.head;
        while (i != index) {
            curr = curr.next;
            i++;
        }
        return curr;
    }
}
```