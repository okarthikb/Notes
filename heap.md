#### What is a heap?

A heap is a type of binary tree. There are two kinds of heaps - min-heap and max-heap. In a min-heap, every all children of a node are greater than than the node. In a max-heap, they're less than the node.

A heap is used in various algorithms like Prim's minimum spanning tree, Dijkstra's algorithm, priority queue, etc. We have a fixed capacity `capacity`, we track the current size `size`, and we have an array `heap` which contains the value of the nodes. If the array is 0-indexed, then the children of node $i$ are at $2i + 1$ and $2i + 2$. The parent of $i$ is at $\lfloor\frac{i - 1}{2}\rfloor$. 

Min-heap is implemented like so...

```c++17
#include <iostream>

using namespace std;

class MinHeap {
	int size;
	size_t capacity;
	int* heap;
	MinHeap(size_t _capacity) {
		size = 0;
		capacity = _capacity;
		heap = new int[_capacity];
		memset(heap, INT_MAX, _capacity);
	}
	int parent(int i) { return (i - 1) / 2; }
	int left(int i) { return 2 * i + 1; }
	int right(int i) { return 2 * i + 2; }
	void push(int x);
	int pop();
	void heapify(int i);
	void delete(int i);
	void decrease(int i, int q);
	void increase(int i, int q);
};
```

Pushing...

```cpp
void MinHeap::push(int x) {
	size++;
	if (size == capacity) {
		cout << "overflow!" << endl;
		return;
	}
	if (size == 1) {
		heap[0] = x;
		return;
	}
	int i = size - 1;
	heap[i] = x;
	while (i != 0 && heap[parent(i)] > heap[i]) {
		swap(&heap[parent(i)], &heap[i]);
		i = parent(i);
	}
}
```

Heapifying...

```cpp
void MinHeap::heapify(int i) {
	int l = left(i), r = right(i), s = i;
	if (l < size && heap[l] < heap[s]) s = l;
	if (r < size && heap[r] < heap[s]) s = r;
	if (s != i) {
		swap(&heap[i], &heap[s]);
		heapify(s);
	}
}
```

Popping...

```cpp
int MinHeap::pop() {
	size--;
	if (size == -1) return -INT_MAX;
	if (size == 0) return heap[0];
	int root = heap[0];
	swap(&heap[0], &heap[size]);
	heapify(0);
	return root;
}
```

Deleting...

```cpp
void MinHeap::delete(int i) {
	if (i < size) {
		size--;
		swap(&head[i], &head[size]);
		heapify(i);
	}
}
```

Increasing...

```cpp
void MinHeap::increase(int i, int q) {
	heap[i] += q;
	heapify(i);
}
```

Decreasing...

```cpp
void MinHeap::decrease(int i, int q) {
	heap[i] -= q;
	while (i != 0 && heap[parent(i)] > heap[i]) {
		swap(&heap[i], &heap[parent(i)]);
		i = parent(i);
	}
}
```

And that's it. These are the method definition for a basic min-heap in C++17.

<br>

#### Complexity analysis

Let $N$ be number of heap elements.

1. Push:
	We loop until $i$ equals 0 worst-case, and since $N$ trees means $log(N)$ levels, and each swap means going up a level, we have $O(log(N))$ steps.

2. Heapify:
	Similar to push, each swap takes us another level (down instead of up), and at index $i$, we have $N - i$ nodes which gives us $O(log(N-i))$ steps.

3. Pop:
	We return the root node value, and then call `heapify(0)` which is $O(log(N-0))=O(log(N))$ steps.

4. Delete:
	We swap the node at index $i$ with final node, and then call `heapify(i)` which is $O(log(N - i))$.

5. Increase:
	Similiar to delete, except we increase node at index $i$ and call `heapify(i)` which is $O(log(N-i))$.

6. Decrease:
	Similiar to push, except we decrease node at index $i$, and if we loop until $i$ equals 0 worst case, we have $O(log(i))$ steps.

