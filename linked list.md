#### What is a linked list?

Linked list consists of a node with a value and a pointer to another node (next node). `next` stores address of next node.

```cpp
#include <iostream>

using namespace std;

struct Node {
	int value;
	Node* next;
	Node(int value):
		value(value),
		next(NULL)
	{}
}
```

Inserting into a liked list...

```cpp
void insert(Node** head, int value) {
	Node* node = new Node(value);
	node->next = *head
	*head = node;
}
```

Inverting a linked list...

```cpp
Node* invert(Node** head) {
	Node* prev = NULL;
	Node* cur = *head;
	Node* next;
	while (next != NULL) {
		next = cur->next;
		cur->next = prev;
		prev = cur;
		cur = next;
	}
	return prev;
}
```

Deleting top node from linked list...

```cpp
int pop(Node* head) {
	if (head) {
		int root = head->value;
		head = head->next;
		return root;
	} else {
		cout << "list empty!";
		return -1;
	}
}
```

Deleting node with value `value` from linked list...

```cpp
void del(Node** head, value) {
	Node* temp = *head;
	while (temp != NULL) {
		if (temp->value == value) {
			*temp = *(temp->next);
			return;
		}
		temp = temp->next;
	}
}
```

<br>

#### Complexity analysis

Let $N$ be number of nodes in the linked list.

1. Insert:
	Since we are simply creating a new node, and changing value of the `head` and `next` pointers, time complexity is $O(1)$.
	
	
2. Invert:
	We are going through all the nodes, so $O(N)$.
	

3. Deleting top node:
	$O(1)$ since we're just changing and returing a value.
	
	
4. Deleting a node with a specific value:
	$O(N)$ like inverting, since we're going through all the nodes.