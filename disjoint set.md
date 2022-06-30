#### What is a disjoint set?

A disjoint is a data structure used to group graph nodes together. It consists of an array where value at index $i$ is the root of $i$.

```cpp
class DisjointSet {
	size_t n;
	int* arr;
	int* size;
	DisjointSet(size_t _n) {
		n = _n;
		arr = new int[_n];
		size = new int[_n];
		for (int i = 0; i < _n; i++) {
			arr[i] = i;
			size[i] = 1;
		}
	}
	int root(int i) {
		return i == arr[i] ? i : arr[i] = root(arr[i]);
	}
	void unite(int i, int j) {
		int u = root(i), v = root(j);
		if (u != v) {
			if (size[u] < size[v]) swap(&u, &v);
			arr[u] = v;
			size[u] += size[v];
		}
	}
};
```

<br>

#### Complexity analysis

See Wikipedia: https://en.wikipedia.org/wiki/Disjoint-set_data_structure