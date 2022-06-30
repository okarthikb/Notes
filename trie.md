#### What is a trie?

A trie is a data structure that is used to analyze strings. It consists of a node, which has two values - one is an array of pointers to children nodes, and a `isEndOfWord` or `eow` boolean flag.

It is implemented like so...

```cpp
#include <iostream>

using namespace std;

struct Node {
	Node* nxt[ALPHABET_SIZE];
	bool eow;
	Node(): eow = false {}
};
```

To insert a string, we use the `insert` method...

```cpp
void insert(Node** node, string str) {
	Node* temp = *node;
	for (char ch: str) {
		size_t i = ch - 'a';
		if (temp->nxt[i] == NULL) temp->nxt[i] = new Node();
		temp = temp->nxt[i];
	}
	temp->eow = true;
}
```

To check if a string is present, we use the `present` method...

```cpp
bool present(Node** node, string str) {
	Node* temp = *node;
	for (char ch: str) {
		size_t i = ch - 'a';
		if (temp->nxt[i] == NULL) return false;
		temp = temp->nxt[i];
	}
	return temp->eow;
}
```

And that's it.

If we have a sentence like "the quick brown fox jumped over the lazy dog", we build up the trie using the words, and then check if a word is present or not.

```cpp
Node* node = new Node();
vector<string> sentence = { "the", "quick", "brown", "fox", "jumped", "over", "the", "lazy", "dog" };
for (string word: sentence) insert(&node, word);
for (string word: sentence) cout << (present(&node, word) ? "true" : "false") << ' ';
```

<br>

#### Complexity analysis

Let $L$ be length of input string. Both methods are $O(L)$ since we're looping through each character once.

