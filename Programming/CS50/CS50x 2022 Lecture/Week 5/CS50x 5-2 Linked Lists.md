# `->`
it's the same as `.` and `*`
* `.` means go into a `struct`
* `*` means go into an address
* `->` denotes going to an address and looking at a field inside of it
they're so commonly used so we have a *syntactic sugar* for them
___

# Linked Lists

from the [[CS50x 5-1 Resize arrays|previous slide]], we learnt we could resize an array by reallocating it inside memory of the heap. the downside is that it would be a slow process since we're copying the whole array to a new location.

actually we do have another approach to resize an array.
which is made possible by:
1. first put the new item to a new location in memory, which may not be contigous to the array.
    at this point the new item is not back-to-back-to-back to the array
2. therefore we need a pointer to tell the computer, after the end of the array, where can it finds the next item

an array like this is called *Linked List*, since it links things together in memory
but actually we would call it **list** instead of array
because items in an *array* must be *contigous*

but what is the trade off?
well, for every number that we had in the list, we need a pointer to show us the next number.
so this approach requires much memory space, every time we use `malloc()`, we didn't just ask room for the number, but also the pointer.
although this approach is faster than our previous array based appraoch.

> [!Quote]
> Almost any time in CS, when we start using more space, we can save time. Or if we try to conserve space, we might have to lose time.
^917ee6

wait, what? isn't the array based approach used twice as much memory, almost the same as this linked list approach?
well, in the array based approach we would just use the memory temporarily. after we're done copying the old array to new array, we can just `free()` them to give them back to the computer

> [!important]
> Anything involving asking for or giving back memory, tends to be slower.

how would a list looks like?
firstly it's important to know that *nodes*[^1] for any data structures are just like *items* for array
ie. 1st node is the first item in list, 2nd node is the 2nd item in list, etc.
| | node 1 | node 2 | node 3 |
| --- | :-: | :-: | :-: |
| **number**| 1 | 2 | 3 |
| **address** | `0x123` | `0x456` | `0x789` |
| **pointer** | `0x456` | `0x789` | `0x0` |
`0x0` is a special value that ancient programmers decided we'll never use this address
this is actually `NULL` ^f382ff

[^1]: whenever we have some data structure that encapsulates information, *node* is how we would call them

how is a node looks like in code? remember how we define a [[CS50x 3-3 Data Structure#Custom Data Structure|person]] in code?
```c
typedef struct
{
	int number;
	node *next;
	// next is just how CS people would name something indicating the next thing
}
node;
```
but actually the compiler will give an error to us if we do it like this because C compiler reads code from top-to-bottom, so at `node *next;` the compiler still don't know what is a `node`

we would do it like this instead:
```c
typedef struct node // this is how we temporarily give a name to a struct
{
	int number
	struct node *next;
}
node; // here we're just shortening the name of `struct node` to `node`
	  // because this is more convenient

// actually we can just use
struct node
{
	int number
	struct node *next;
};
// but this way we would type `struct node` every time we use it
// it would not be efficient though
```

why it is `node *next`, not `int *next`?
this way the computer would just read the int part from the nodes, rather than the whole node. it would not know where is the next node since we threw away that information.

and then how would we use `node`?
```c
int main(void)
{
	// declare a empty list
	node *list = NULL; // we assign to it NULL to avoid pointing to a garbage value
	
	// add a new number to the list
	node *n = malloc(sizeof(node));
	if (n != NULL)
	{
		(*n).number = 1;
		// we need (*n) because we want to go to n first before accessing number
		// thankfully we have ->, so we can do this instead
		n->number = 1;
		
		// also we have to assign to *n the next node
		n->next = NULL;
	}
	// actually we're not done adding the number 1 yet
	// remember `n` is what we created temporarily for a new node
	// `list` is our actual linked list, so we need to assign to it `n`
	list = n;
	// `n` would just go away eventually?
}
```

let's see another example, where we have 4 numbers in the list:
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct node
{
	int number;
	struct node *next;
}
node;

int main(void)
{
	// List of size
	node *list = NULL;
	
	// Add a number to list
	node *n = malloc(sizeof(node));
	if (n == NULL)
	{
		return 1;
	}
	n->number = 1;
	n->next = NULL;
	
	// Update list to point to new node
	list = n;
	
	// Add a number to list
	n = malloc(sizeof(node));
	if (n == NULL)
	{
		free(list);
		return 1;
	}
	n->number = 2;
	n->next = NULL;
	list->next = n;
	
	// Add a number to list
	n = malloc(sizeof(node));
	if (n == NULL)
	{
		// remember that we should free the list in reverse order
		// starting from the last node
		// otherwise computer can't know where is `list->next`?
		free(list->next);
		free(list);
		return 1;
	}
	n->number = 3;
	n->next = NULL;
	list->next->next = n;
	
	// Can't print numbers
	for (int i = 0; i < 3; i++)
	{
		printf("%i\n", list[i]);
	}
	// actually this would not work because `[i]` would trust that
	// the datas are stored back-to-back-to-back, which is not this case
	
	// Print numbers
	for (node *tmp = list; tmp != NULL; tmp = tmp->next)
	{
		printf("%i\n", tmp->number); // remember `tmp->number` is (*tmp).number
	}
	// 1. init a node pointer `tmp` which has the address same as `list`
	// 2. for each iteration, does `tmp` not equal `NULL`?
	// 3. print `tmp->number`
	// 4. grab next node of `tmp` and put it in `tmp`
	// 5. repeat until `tmp` is `NULL`
	
	// Can't free list
	free(list);
	// if the world of array we could do just like this
	// for linked list though the addresses are, again, not contigous
	
	// Free list
	while(list != NULL)
	{
		node *tmp = list->next; // `tmp` is the next node
		free(list);             // we free `list`, which points to the current node
		list = tmp;             // `list` now points to the same location as `tmp`
	}
	return 0;
}
```

but no doubt this example set the list very manually
it would be better if we do this with a loop and some functions that are automating this process

also notice that in this approach we `malloc()` each node one at a time
unlike how we would `malloc()` the whole array at a time in the [[CS50x 5-1 Resize arrays#^b3ef0e|array based approach example]]
___

# Pitfall of linked lists

imagine now we would have a new node, maybe with a number `0`
we would like it to be the first node, what we need to do?
we need to update `list` to make it points to our new node right?

nope. if we do so, how about all the others nodes once we do that?
from the computer's point of view, where are they now?
well, they're lost now in the memory ocean since **nothing** is pointing at them any more
a [[C - 11 Dynamic memory allocation#memory leak|memory leak]] happen

this is the danger of using pointers, and dynamic memory allocation
thus now it becomes the danger of building our own data structures too

what we need to do is: ^52039f
1. assign an node to our new node's `next`
    that node should be the node the last node of the new node points to
2. update the last node to make it points to the new node

```c
// incorrect example
list = n;

// correct example
n->next = list;
```
___

# Running time of linked lists

Search
* $O(n)$
* we need to start from the first node any how, otherwise we can't look through all nodes
* the process is *linear*
* since we can't jump to the middle of the list, we can't do [[CS50x 3-2 Linear and Binary Search#Binary Search|Binary Search]], which is very powerful and have a running time of $O(n\log{}n)$

Insert
* $O(1)$, if we don't care where the new node should be
* $O(n)$, if the list need to be keep sorted
* $\Omega(1)$
___

# Win?
Is linked list a clear win over array?
nope.

Wins
* we don't need a massive growing chunk of contiguous memory
* if we resize the list, it would be easier than resizing an array

Loses
* we need memory for the pointers
* we still need $O(n)$ times to find the end of it
* we don't have random access to the element any more
___

# using a Singly-Linked List

**Create a linked list**
we may want a function to create a node, something like `sllnode* create(int num);`

that does:
1. Dynamically allocate space for a new `sllnode`
2. Check to make sure we didn't run out of memory
3. Initialize the `sllnode`'s fields
    including the `next` pointer to make it points to the next node, or points to `NULL` when there is no next node
4. Return a pointer to the newly created `sllnode`


**Find an element**
similarly, we may want a function to search the list for an element
something like `bool find(sllnode* head, VALUE val);`
where `head` is the very first element of the list, and `val` is the value that we're looking for

in fact, do always keep track of the very first element of the list, even put it into a global variable, that way we always can refer to all the other elements without having to keep track of every pointer in all the `sllnode`
and we just need to keep track of the first one

the function does:
1. Create a *traversal pointer* pointing to the list's head
    it just means duplicate the pointer at the list's head, so later we will move the duplicate instead of the first pointer itself
2. see if the current `sllnode` has the value `val`
3. if yes, report success
4. if no, move the *traversal pointer* to the next pointer in the list and repeat from 2.
5. until we have reached the end of the list, report failure


also we don't need to `malloc()` for the *traversal pointer*. that would be incorrect.
since that space in memory already exists, we just need another pointer, not additional space, that points to the same space

in addition it may seems it's better to perseve our lists in order
although it does matter as linked lists don't have index associated with its nodes
even if we have a sorted linked list, there is no way we can search through the list other than using a linear search


**Insert a new node**
a function for that may look like `sllnode* insert(sllnode* head, VALUE val);`
that does:
1. Dynamically allocate space for a new `sllnode`
2. Check to make sure we didn't run out of memory
3. Populate and insert the node at the beginning of the linked list
	* assign values to the `sllnode`'s fields
	* insert at the beginning so it's immediate, if we insert at the end we would have to move from the beginning of the list to the end of the list linearly, and then finally tack it on
		* it's $O(1)$ verse $O(n)$
4. Return a pointer to the new head of the linked list

do remember to make the new `sllnode` points to the original head of the list first, before making the newly created `sllnode` the new head of the list
otherwise the original list would be lost and gone from the the computer's sight


**Delete an entire linked list**
its function would be like `void destroy(sllnode* head);`
that does:
1. if the current node's `next` pointer is not `NULL`, get in there
2. repeat 1. until at a point a node's `next` is `NULL`
3. delete the current node, and get back to the previous node
4. repeat 3. until the head node is `free()`


**Delete a single element**
it is a bit tricky because we have to skip over a node in the list, making the last node connects to the next node, so that we don't lose the rest of the list
when we get to the node that we want to delete, we need to be able to step backward 1 node
but a singly-linked list doesn't allow us to do that, so we either need 2 pointers, one behind the other as we move them, or get to a point and then send another pointer through.
therefore this operation is easier on doubly-linked list.
___

# Doubly-Linked List
unlike *singly-linked list* that only allows us to move forward through the list
a *doubly-linked list* allows us to move both forward and backward through the list, by simply adding one extra pointer to each node

if we're not going to be deleting single element from the list, it would be better to stick with *singly-linked list* to save some memory
if we do need to delete any single element though, *doubly-linked list* would be a better choice
also some other certain operations could benefit from a *doubly-linked list* too

```c
typedef struct dllist
{
	VALUE val;
	struct dllist* prev;
	struct dllist* next;
}
dllnode;
```

Creating an initial list / Search through the list / Delete the entire list are just the same as the case in *singly-linked list*
It's only inserting a new node and deleting a single element are different


**Insert a new node**
the function is similar to the *singly-linked list*'s case though:
`dllnode* insert(dllnode* head, VALUE val);`
that does:
1. Dynamically allocate space for a new `dllnode`
2. Check to make sure we didn't run out of memory
3. Populate and insert the node at the beginning of the linked list
4. Fix the `prev` pointer of the old head of the linked list
5. Return a pointer to the new head of the linked list

again, be careful not to break the chain when rearranging the pointers
the rearrangement needs to be done in correct order
sometimes we may even need to thave redundant pointers temporarily for the operations, but that's okay

again, insertion at the beginning of the list has a **constant** running time.


**Delete a node**
the function to find a node and delete it would be like this:
`void delete(dllnode* target);`
that does:
1. Fix the pointers of the surrounding nodes, so none of them points to our target node any more, they should "skip" our target node and points to each other
	* in the case that our target node is actually the very beginning node or the end node of the list, it would be a bit different, but you get the idea
2. `free()` `target`

notice this operation has a **constant** running time, it doesn't matter how larget the list is
we just move the 2 pointers from the `next` and `prev` node, and then `free()` the `target`


**Searching a node** though, takes **linear** running time to do.
___
