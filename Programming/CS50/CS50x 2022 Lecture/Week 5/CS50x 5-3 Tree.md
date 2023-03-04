how about instead of just stringing together things laterally, left to right, using [[CS50x 5-2 Linked Lists|linked list]], we leverage (= make use of) a second dimension.
___

# Binary Search Trees

remember what [[CS50x 3-2 Linear and Binary Search#Binary Search|Binary Search]] requires?
1. items are sorted
2. items' addresses are contiguous

and how the search works?
| | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| --- | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 1st try | | | | 4 | | | |
| 2nd try | | 2 | | | | 6 | |
| 3rd try | 1 | | 3 | | 5 | | 7 |

what it looks like? what it could be?
[[CS50x 5-2 Linked Lists#^f382ff|nodes]]! they still store an int.
but this time we connect them using not just one pointer, but as many as two.
nodes at the bottom, ie. `1, 3, 5, 7`, they are the leaves, that have 0 pointer

> [!tip]
> `4` would be called the *root* of the tree, or a *parent*, with respect to its *children* `2, 6`
> `1, 3, 5, 7` would be the *grand-children* of `4`, or called the *leaves*
> `2, 6` or `1, 3, 5, 7` would be the *siblings* with respect to each other

how a node looks like in code?
```c
typedef struct node
{
	int number;
	struct node *left;
	struct node *right;
}
node;
```

since for every node, number in the left node is smaller than the number in the parent node, and number in the right node is greater than the number in the parent node
we can apply [[CS50x 3-2 Linear and Binary Search#Binary Search|Binary Search]]
then it gives us $O(n\log_2{n})$ running time to search for a number

the price is that we're using even more memory for each node now
again, there is a [[CS50x 5-2 Linked Lists#^917ee6|trade-off]] between time and space
> [!quote]
> There is always a price paid. And it's very often in space, or time, or complexity, or developer time, the number of bugs we have to solve.

here is a code that manually set a tree of 3 nodes:
```c
// Implements a list of numbers as a binary search tree

#include <stdio.h>
#include <stdlib.h>

// Represents a node
typedef struct node
{
	int number;
	struct node *left;
	struct node *right;
}
node;

void free_tree(node *root);
void print_tree(node *root);

int main(void)
{
	// Tree of size 0
	node *tree = NULL;
	
	// Add number to list
	node *n = malloc(sizeof(node));
	if (n == NULL)
	{
		return 1;
	}
	n->number = 2;
	n->left = NULL;
	n->right = NULL;
	tree = n;
	
	// Add number to list
	n = malloc(sizeof(node));
	if (n == NULL)
	{
		// Free memory
		return 1;
	}
	n->number = 1;
	n->left = NULL;
	n->right = NULL;
	tree->left = n;
	
	// Add number to list
	n = malloc(sizeof(node));
	if (n == NULL)
	{
		// Free memory
		return 1;
	}
	n->number = 3;
	n->left = NULL;
	n->right = NULL;
	tree->right = n;
	
	print_tree(tree);
	
	// Free tree
	free_tree(tree);
}

void print_tree(node *root)
{
	if (root = NULL)
	{
		return; // not `return 0`, since the function has a `void` output type
	}
	print_tree(root->left);
	printf("%i\n", root->number);
	print_tree(root->right);
}
// notice how there is a recursion going on?
// it's a good use of recursion since the data structure itself is recursive
// also we could do printing the right nodes first in this approach
// just by moving the 2 `print_tree()` functions in reverse positions

void free_tree(node *root)
{
	if (root == NULL)
	{
		return;
	}
	free_tree(root->left);
	free_tree(root->right);
	free_tree(root);
	// it's important that we only release `root`
	// after both the `left` and `right` nodes are freed
	// because if `root` is already gone, `left` and `right` nodes are also gone
	// then we would not be able to `free()` them
}
```

now how would we search a binary tree?
```c
bool search(node *tree, int number)
{
	if (tree == NULL)
	{
		return false;
	}
	else if (number < tree->number)
	{
		return search(tree->left, number);
	}
	else if (number > tree->number)
	{
		return search(tree->right, number);
	}
	// else if (number == tree->number) // don't ask meaningless question
	else
	{
		return true;
	}
}
```
___

# Running times of binary search tree

firstly, is this a binary tree?
| | 1 | 2 | 3 |
| --- | :-: | :-: | :-: |
| 1st try | | 2 | |
| 2nd try | 1 | | 3 |
yes. the left child is smaller than the root and the right child is greater than the root.

now is this a binary tree?
| | 1 | 2 | 3 |
| --- | :-: | :-: | :-: |
| 1st try | 1 | | |
| 2nd try | | 2 | |
| 3rd try | | | 3 |
yes for sure. the left child, which doesn't exist, is smaller than the roots and the right childs are greater than their parents.

however, what does this look like?
[[CS50x 5-2 Linked Lists|Linked list]], at a different axis

in the last example, we inserted `2` first, and then `1` and `3`, so it allows us to do binary search
in this example though, we just do insert `1` and then `2` and then `3`. although the outcome still can be described as a *binary tree*, binary search really can't do anything here.

we need to kind of "rotate" the tree to make binary serach possible
and there do are trees that can help us pivot a tree and rotate it, and kind of snip off the root and fix things.
eg. *AVL trees*, *red-black trees*, etc.

now come back to the question of what are the running time?

**search / insert**
* $O(\log{n})$, if the nodes are arranged ideally
* $O(n)$, if the nodes are arranged bad
___
