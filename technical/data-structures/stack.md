```Java
/*
it should look something like this
	[A]<-[B]<-[C]h
push adds to head
pop removes from head
 */
interface IStack<T> {
    void push(T data);
    T pop();
    T peek();
}

public class Stack<T> implements IStack<T> {
	Node<T> head;
	public int length;

	class Node<K> {
		K data;
		Node<K> next;

		Node(K data) {
			this.data = data;
			this.next = null;
		}
	}

	@Override
	public void push(T data) {
		Node<T> node = new Node<T>(data);
		node.next = this.head;
		this.head = node;

		this.length++;
	}

	@Override
	public T pop() {
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