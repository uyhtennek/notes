# trie
* short for *retrieval*
* it's a tree that can give us contant time lookup even for massive data sets
* it's an array of tree
	* each node in a trie points to another node, which is to say another array of tree
	* again, using the [[CS50x 5-4 Hash Table#^ecb8c1|previous example]], this time each node has an array of node representing the letters A through Z

say we want to store the name `Hagrid`, how it goes in a trie?
to **insert** an element into a trie, simply build the correct path from the root to the leaf.
| array | A | B | C | D | ... | H | I | ... | Z |
| :-: | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1st node | | | | | | points to 2nd node | | |
| 2nd node | points to 3rd node | | | | | | | | |
| 3rd node | | | | | G points to 4th node | | | | |
| 4th node | | | | | | | | R points to 5th node | |
| 5th node | | | | | | | points to 6th node | | |
| 6th node | | | | `true` | | | | | |
![[trie-hagrid.png]]
in the last node, at the location of `D` we set a `true` value, indicating that the name stop here

similar for **searching** a name, the name we need to search now is a roadmap, we follow it and see if we can get to the end.
if we can, that mean the data exists, vice versa.

and then here is when things get interesting that we store 3 names, all start with the letter H
![[trie-3-names.png]]
we can see some of the names, share a common prefix
and this is compelling because we're reusing space
eg. we used the nodes H and A for both names started with Hag and Har

again, every names are guaranteed to have unique roadmaps through this data structure

let's look at how an node in a trie is represented in code:
```c
typedef struct node
{
	bool is_word;
	// true = a name stops at this node
	// false = this node is just a path to the rest of the name
	struct node *children[SIZE_OF_ALPHABET]; // SIZE_OF_ALPHABET = 26
	// each node stores 26 node pointers
}
node;
```

the upside is that
* the height of a trie is only as tall as the longest person's name
* even if there are 3 millions names in our data sets, it would take how many steps for us to search for `Hermoine`?
	* 8 steps. it's constant.
* running time of **search / insert** is $O(1)$
	* independent to how many pieces of data are already in the data structure
* no collisions. no two pieces of data have the same path, unless they are identical.

also we can have a name that has a prefix of another name
eg. `Daniel` and `Danielle` are both possible to store in a trie

and then, like every other cases, what is its downside?
* it's an incredibly wide data structure, that would have many pointers, thus it would use a huge amount of memory to store a name
* and again, many pointers may not be useful and just points to `NULL`
___

# example: universities data

let's map key-value pairs where:
* keys are 4-digit years `YYYY`
* values are names of universities founded during those years

the paths from a central *root* node to a *leaf* node, where the school names would be, would be labeled with digits of the year

each node on the path from *root* to *leaf* could have 10 pointers emanating ( 散發 ) from it, one for each digit

firstly let's define a node called a `trie`
```c
typedef struct _trie
{
	char university[20];      // the name of a university
	struct _tire* paths[10];  // an array of pointers to other nodes
}
trie;
```

then we need to have a *root* node
we probably would want to globally declare it and globally maintain a pointer to this node
something like `_trie *root = malloc(sizeof(_trie));`
when ever we want to navigate through the trie, we should declare another pointer to be equal to `root`, typically named `trav` for *traversal*, then we use `trav` to navigate through the trie

here is how a data, with a key `1636` and a value `"Harvard"`, would look like in the trie
![[trie-harvard.png]]

next, we insert 2 more data, which are `"Yale"` founded `1701` and `"Dartmouth"` founded `1769`
![[trie-universities.png]]

to **search** `"Harvard"` founded `1636`
1. see if can get down to node 1, if so then get down
2. see if can get down to node 6, if so then get down
3. see if can get down to node 3, if so then get down
4. see if can get down to node 6, if so then get down
5. the key is exhausted, so now see if the value `"Harvard"` is there at the current node
6. the value is confirmed so yes we found `"Harvard"`

the 5. step is important because even when we get to exhaust the key, the value may not be the one we're looking for

to **search** `Princeton` founded `1746`
1. see if can get down to node 1, if so then get down
2. see if can get down to node 7, if so then get down
3. see if can get down to node 4, wait it's pointing to `NULL`
4. it's an dead end so we can say nope `"Princeton"` isn't in this data structure
___

# Summary

notice there are 2 main benefits of using *tries*:
1. **insertion, lookup, deletion** are **constant** time operations
2. as the trie gets bigger and bigger, we have do less work every time to insert new things since we have already built a lot of branches, done a number of `malloc()` already

the tradeoff being that tries are very huge
each one of these nodes takes up a lot of space
___
