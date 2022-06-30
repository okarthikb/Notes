#### What is a binary tree?

Binary tree is a DS where each node has two children which are also binary trees.

```cpp
#include <iostream>
#include <queue>

using namespace std;

struct Node {
	int value;
	Node* left;
	Node* right;
	Node(int value):
		value(value),
		left(NULL),
		right(NULL)
	{}
}
```

Inserting in level order, i.e., inserting element at first `NULL` node.

```cpp
void insertLevelOrder(Node** rootptr, int value) {
	if (*rootptr == NULL) *rootptr = new Node(value);
	queue<Node*> q;
	q.push(*rootptr);
	while (!q.empty()) {
		Node* cur = q.front();
		q.pop();
		if (cur->left == NULL) {
			cur->left = new Node(value);
			return;
		} else q.push(cur->left);
		if (cur->right == NULL) {
			cur->right = new Node(value);
			return;
		} else q.push(cur->right);
	}
}
```

Inserting in order, i.e., elements < root on root's left, and elements >= root on root's right.

```cpp
Node* insertInOrder(Node* root, int value) {
	if (root == NULL) {
		root = new Node(value);
	} else if (value < root->value) {
		root->left = insertInOrder(root->left, value);
	} else {
		root->right = insertInOrder(root->right, value);
	}
	return root;
}
```

Traversing in level order...

```cpp
void travLevelOrder(Node** rootptr) {
	queue<Node*> q;
	q.push(*rootptr);
	while (!q.empty()) {
		Node* cur = q.front();
		q.pop();
		cout << cur->value << " ";
		if (cur->left != NULL) q.push(cur->left);
		if (cur->right != NULL) q.push(cur->right);
	}
}
```

Traversing in order...

```cpp
void travInOrder(Node* root) {
	if (root != NULL) {
		travInOrder(root->left);
		cout << root->value << " ";
		travInOrder(root->right);
	}
}
```

Traversing pre order...

```cpp
void travPreOrder(Node* root) {
	if (root != NULL) {
		cout << root->value << " ";
		travPreOrder(root->left);
		travPreOrder(root->right);
	}
}
```

Traversing post order...

```cpp
void travPostOrder(Node* root) {
	if (root != NULL) {
		travPostOrder(root->left);
		travPostOrder(root->right);
		cout << root->value << " ";
	}
}
```

<br>

#### Complexity analysis

Let $N$ be the number of nodes in the binary tree.

1. Level-order methods:
	We use a while loop to go through all the nodes and arrive at the first empty node where we do some operation. So time complexity is $O(N)$.


2. Pre-order and post-order methods:
	We recursively go through nodes, and each recursive call goes down a level. Since max # levels possible is $\lfloor log(N)\rfloor$, time complexity is $O(log(N))$.