>news shock: most data-structures are graphs, i.e; tree, linked-list, etc...

### graph terms
- cycle: graph traversal starting from a node and return back to starting node, requires at least 3 nodes to be a cycle, i.e; A,B,C,A
- acyclic: a graph that contains no cycle
- connected: every node has a path to another node
- directed: When there is a direction between the connections
- undirected: !directed
- weighted: edges have weight, something like a highway i.e; A -> B has 10 units, B -> A has 20 units
- node: a point or vertex on the graph
- edge: the connections between two nodes

>BigO is measured in terms of V(vertices) and E(edges)

### adjacency list
>a list of node relations, think of it as an array of objects

example
```js
graph = [
			[{to: "B", w: 1}, {to: "C", w: 2}],
			[{to: "A", w: 1}, {to: "C", w: 4}],
			[{to: "A", w: 2}, {to: "B", w: 4}],
		]
```

### adjacency matrix
>an array of arrays, consume a ton of memory, depending on the number of nodes, an example will be for vibration analysis, where each node represents the degree of freedom

example
```python
graph = [
			[0, 0, 1, 1, 0, 0, 0],
	        [0, 0, 1, 0, 0, 1, 0],
	        [1, 1, 0, 1, 1, 0, 0],
	        [1, 0, 1, 0, 0, 0, 1],
	        [0, 0, 1, 0, 0, 1, 0],
	        [0, 1, 0, 0, 1, 0, 1],
	        [0, 0, 0, 1, 0, 1, 0]
	    ]
```
