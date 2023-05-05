```Java
/*
it should look something like this:
	[A]->[B]->[C] or [A]<-[B]<-[C]
adding or removing is constant time, but the traversal is O(n) generally
*/
interface ILinkedList<T> {
   public int length();
   public void insertAt(T data, int index);
   public T remove(T data);
   public T removeAt(int index);
   public void append(T data);
   public void prepend(T data);
   public T get(int index);
}

public class LinkedList<T> implements ILinkedList<T> {
    Node<T> head;
    Node<T> tail;
    int length;

    public LinkedList(T data) {
		this.head = this.tail = null;
		this.length = 0;
    }

    class Node<K> {
        K data;
        Node<K> next;

        public Node(K data) {
            this.data = data;
            this.next = null;
        }
    }

    public int length() {
        return this.length;
    }

    public void insertAt(T data, int index) {
        Node<T> node = new Node<T>(data);
        Node<T> prev = this.traversal(index - 1);
        Node<T> next = prev.next;
        prev.next = node;
        node.next = next;
        this.length++;
    }

    public T remove(T data) {
        if (this.length == 0) {
            return null;
        }
        Node<T> curr = this.head;
        Node<T> prev = null;
        while (curr.data != data && curr != null) {
            prev = curr;
            curr = curr.next;
        }
        if (curr == null) {
            return null;
        }
        prev.next = curr.next;
        curr.next = null;
        this.length--;

        return curr.data;
    }

    public T removeAt(int index) {
        if (this.length == 0 || index > this.length) {
            return null;
        }
        Node<T> prev = traversal(index - 1);
        Node<T> node = prev.next;
        prev.next = node.next;
        node.next = null;
        this.length--;

        return node.data;
    }

    public void append(T data) {
        Node<T> node = new Node<T>(data);
        if (this.length == 0) {
            this.head = node;
            this.tail = node;
        }
        this.tail.next = node;
        this.tail = node;
        this.length++;
    }

    public void prepend(T data) {
        Node<T> node = new Node<T>(data); 
        if (this.length == 0) {
            this.head = node;
            this.tail = node;
        }
        node.next = this.head;
        this.head = node;
        this.length++;
    }

    public T get(int index) {
        if (index > this.length) {
            return null;
        }
        Node<T> node = this.traversal(index);
        return node.data;
    }

	// this can be improved using algorithms
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