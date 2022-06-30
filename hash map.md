#### What is a hash map?

A hash map is a data structure in which there are key-value pairs. Each key is passed into a hash function, which returns a hash, which returns an index (on modulo $m$). The value is stored at the index. Sometimes, two keys can result in the same hash - this is called hash collision. To deal with this, a simple solution is to use a linked list at every index (instead of a single entry, we have multiple entries). So if there's already a value stored at index $i$, we can store another value by just appending to the linked list at index $i$.

```cpp
#include <iostream>

using namespace std;

struct Node {
	int key;
	int value;
	Node* next;
	Node(int key, int value):
		key(key),
		value(value),
		next(NULL)
	{}
};

void _insert(Node** node, int key, int value) {
	Node* temp = *node;
	while (temp != NULL) {
		if (temp->key == key) {
			temp->value = value;
			return;
		}
	}
	temp = new Node(key, value);
	temp->next = *node;
	*node = temp;
}

void _del(Node** node, int key) {
	Node* temp = *node;
	Node* prev = NULL;
	while(temp != NULL && temp->key != key) {
		prev = temp;
		temp = temp->next;
	}
	if (temp == NULL) return;
	prev->next = temp->next;
	delete temp;
	return;
}

int _get(Node** node, int key) {
	Node* temp = *node;
	while (temp != NULL) {
		if (temp->key == key) return temp->value;
		temp = temp->next;
	}
	return -1;
}

class HashMap {
	public:
	const static size_t size = 32;
	Node* map[size];
	HashMap() {
		for (size_t i = 0; i < size; i++) map[i] = NULL;
	}
	void insert(int key, int value);
	void del(int key);
	int get(int key);
};

void HashMap::insert(int key, int value) {
	_insert(&map[key % size], key, value);
}

void HashMap::del(int key) {
	_del(&map[key % size], key);
}

int HashMap::get(int key) {
	return _get(&map[key % size], key);
}
```

<br>

#### Complexity analysis

1. Insert:
	Suppose max value of key is $N$ and $s$ is size of hash map. Then max size of linked list at each index is $\frac{N}{s}$, and max insertion time is $\frac{N}{s}$ which is constant, i.e., $O(1)$.
	

2. Get:
	$O(1)$, since access time is $O(1)$.
	

3. Delete:
	Also $O(1)$, since access time is $O(1)$.