# DFS (O(N/2))
>preserves shapes/structure, uses a stack
```java
public boolean dfs(Node<T> node, T value) {
	if (node == null) {
		return false;
	}

	if (node.value.equals(value)) {
		return true;
	}

	// this makes it O(N) but technically it should only search one side in bst
	return this.dfs(node.left, value) || this.dfs(node.right, value);
}
```

# BFS (O(logN))
>uses a queue
```java
public boolean bfs(Node<T> node, T value) {

	final Queue<Node<T>> q = new LinkedList<Node<T>>();
	q.add(node);

	while (q.size() > 0) {
		Node<T> curr = q.remove();

		if (curr.value.equals(value)) {
			return true;
		}

		if (curr.left != null) {
			q.add(curr.left);
		}

		if (curr.right != null) {
			q.add(curr.right);
		}
	}

	return false;
}
```

# Dijkstra

requirements:
- DAG
- +ve weights

steps:
1. select a source node as current node
2. assign source node with distance of 0 and other nodes with distance of infinity
3. check neighboring nodes distance from current node
4. if node distance is less than prev distance then update the node in a min heap and the predecessor list
5. pop the min heap to move the current node
6. repeat step 3 - 5 until all nodes have been visited
7. the shortest path can be build by selecting a destination node and follow the predecessor list backwards

# Bellman-Ford

requirements:
- DAG

steps:
1. select a source node
2. assign source node with distance of 0 and other nodes with distance of infinity
3. select any node in the graph as current node
4. do a sweep from current node to update the distance of other nodes
5. if the distance between nodes is less than the current distance then update the node distance and update the predecessor list
6. if there is an update of any node then continue doing step 4-5
7. the shortest path can be build by selecting a destination node and follow the predecessor list backwards

# A*

cons:
- does not guarantee shortest path

# IDA*