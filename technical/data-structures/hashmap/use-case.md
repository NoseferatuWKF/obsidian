lru-cache
```go
package ds

type Node[T any] struct {
    Value   T
    Next    *Node[T]
    Prev    *Node[T]
}

type LRU[K comparable, V any] struct {
    Capacity    int32
    Length      int32
    Head        *Node[V]
    Tail        *Node[V]
    Lookup      map[K]*Node[V]
    RLookup     map[*Node[V]]K
}

func (lru *LRU[K, V]) Init(c int32) {
    lru.Capacity = c
    lru.Length = 0
    lru.Lookup = make(map[K]*Node[V])
    lru.RLookup = make(map[*Node[V]]K)
}

func (lru *LRU[K, V]) Update(key K, value V) {
    node := lru.Lookup[key]
    
    if node == nil {
        node = &Node[V]{}
        node.Value = value

        lru.Length++
        lru.prepend(node)
        lru.trimCache()

        lru.Lookup[key] = node
        lru.RLookup[node] = key
    } else {
        node.Value = value

        lru.detach(node)
        lru.prepend(node)
    }
}

func (lru *LRU[K, V]) Get(key K) V {
    node := lru.Lookup[key]

    if node == nil {
        return *new(V)
    }

    lru.detach(node)
    lru.prepend(node)

    return node.Value
}

func (lru *LRU[K, V]) detach(node *Node[V]) {
    if node.Prev != nil {
        node.Prev.Next = node.Next
    }
    
    if node.Next != nil {
        node.Next.Prev = node.Prev
    }

    if lru.Head == node {
        lru.Head = lru.Head.Next
    }

    if lru.Tail == node {
        lru.Tail = lru.Tail.Prev
    }

    node.Next, node.Prev = nil, nil
}

func (lru *LRU[K, V]) prepend(node *Node[V]) {
    if (lru.Head == nil) {
        lru.Head, lru.Tail = node, node
        return
    }

    node.Next = lru.Head
    lru.Head.Prev = node
    lru.Head = node

}

func (lru *LRU[K, V]) trimCache() {
    if lru.Length <= lru.Capacity {
        return
    }

    tail := lru.Tail
    lru.detach(lru.Tail)

    key := lru.RLookup[tail]
    delete(lru.Lookup, key)
    delete(lru.RLookup, tail)
    lru.Length--
}
```