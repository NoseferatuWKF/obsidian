```Java
/*
it should look something like this
	h[A]->[B]->[C]t
enqueue adds to tail
deque removes from head
 */
interface IQueue<T> {
    void enqueue(T data);
    T deque();
    T peek();
}

public class Queue<T> implements IQueue<T> {
	public int length;
	Node<T> head;
	Node<T> tail;

	class Node<K> {
		K data;
		Node<K> next;

		Node(K data) {
			this.data = data;
			this.next = null;
		}
	}

	public Queue() {
		this.head = this.tail = null;
		this.length = 0;
	}

	@Override
	public void enqueue(T data) {
		Node<T> node = new Node<T>(data);
		if (this.length == 0) {
			this.head = this.tail =  node;
		} else {
			this.tail.next = node;
			this.tail = node;
		}

		this.length++;
	}

	@Override
	public T deque() {
		Node<T> node = this.head;
		this.head = node.next;
		node.next = null;

		this.length--;
		return node.data;
	}

	@Override
	public T peek() {
		return this.head.data;
	}

}
```