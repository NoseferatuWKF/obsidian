>aka priority queue

>root/top value must be smallest

>every node below must be larger (or technically equal to)

>2i+1(left child), 2i+2(right child), (i-1)/2(parent), length(last index)

```java
import java.util.List;
import java.util.ArrayList;

interface IMinHeap {
    public void insert(int value);
    public Integer delete();
}

public class MinHeap implements IMinHeap {

    public int length;
    public List<Integer> data;

    public MinHeap() {
        this.length = 0;
        this.data = new ArrayList<Integer>();
    }

	@Override
	public void insert(int value) {
        this.data.set(this.length, value);
        this.heapifyUp(this.length);
        this.length++;
	}

	@Override
	public Integer delete() {
        if (this.length == 0) {
            return -1;
        }

        final Integer out = this.data.get(0);

        if (this.length == 1) {
            this.data = new ArrayList<Integer>();
            this.length--;
            return out;
        }

        this.length--;
        this.data.set(0, this.data.get(this.length));
        this.heapifyDown(0);
        return out;
	}

    private int parent(int index) {
		// maybe need to do ceil
        return (index - 1) / 2;
    };

    private int leftChild(int index) {
        return index * 2 + 1;
    };

    private int rightChild(int index) {
        return index * 2 + 2;
    };

    private void heapifyUp(int index) {
        if (index == 0) {
            return;
        }

        int p = this.parent(index);
        Integer pV = this.data.get(p);
        Integer v = this.data.get(index);

        if (pV > v) {
            this.data.set(p, v);
            this.data.set(index, pV);
            heapifyUp(p);
        }
    }

    private void heapifyDown(int index) {
        final int lIdx = this.leftChild(index);
        final int rIdx = this.rightChild(index);

        if (index >= this.length || lIdx >= this.length) {
            return;
        }

        Integer lV = this.data.get(lIdx);
        Integer rV = this.data.get(rIdx);
        Integer v = this.data.get(index);

        if (lV.compareTo(rV) > 0 && v.compareTo(rV) > 0) {
            this.data.set(rIdx, v);
            this.data.set(index, rV);
            this.heapifyDown(rIdx);
        } else if (rV.compareTo(lV) > 0 && v.compareTo(lV) > 0) {
            this.data.set(lIdx, v);
            this.data.set(index, lV);
            this.heapifyDown(lIdx);
        }
    }

}

```