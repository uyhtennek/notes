# 4 basic data structures

at the end of the day, every data structures can eventaully be boiled down to 4 basic ideas:
1. [[C - 8 Arrays|Arrays]]
2. Linked lists
3. Hash tables
4. Tries

for others data strucutres
* [[CS50x 5-3 Tree|trees]] and heaps, are quite similar to tries
* [[CS50x 5-6 abstract data structures|stacks and queues]], are quite similar to arrays or linked lists
___

# How to choose data structures?

**cheat sheet**
| | Insertion | Deletion | Lookup | Sorting | Size in memory |
| --- | :-: | :-: | :-: | :-: | :-: |
| Arrays | bad | bad | great | easy | small |
| Linked Lists | easy | easy | bad | bad | small |
| Hash Tables | easy | easy | good | can't do | big |
| Tries | constant time | constant time | constant time | no need | very big |


**Arrays**
* Insertion is bad
	* lots of shifting to fit an element in the middle
	* insertion at the end of the array is okay though, if we're building the array as we go
* Deletion is bad
	* lots of shifting after removing an element, if we don't want to leave empty gaps, which usually we don't want to, since it's wasting memory
	* again, unless we're deleting at the end of the array
* Lookup is great
	* have random access, contant time operation
	* if we say 7, it goes to the location 7. if we say 20, it goes to location 20.
	* we don't need to iterate across
* Relatively easy to sort
	* for all [[CS50x 3-4 Sorting#Selection Sort|selection sort]], insertion sort, [[CS50x 3-4 Sorting#Bubble Sort|bubble sort]], and [[CS50x 3-4 Sorting#Merge Sort|merge sort]]
* Relatively small size-wise
* Stuck with a fixed size, no flexibility

**Linked lists**
* Insertion is easy
	* just tack onto the front
* Deletion is easy
	1. find the element
	2. update the last node to points to the next node
	3. `free()` the node
* Lookup is bad
	* have to use linear search
	* don't have random access
* Relatively difficult to sort
	* we can't really sort a linked list, unless we sort it as we construct it
		* imagine updating every nodes to point in sorted order, it would be a nightmare
	* but then if we sort it as we construct it, that means we have to find the right spot before inserting an element, then it would be as bad as inserting into an array
* Relatively small size-wise
	* larger than arrays
	* but not a huge amount of wasted space

**Hash tables**
* Insertion is a 2 step process - *hash* then *add*
	* first we need to run our data through a hash function to get a hash code
	* then we insert the element into the hash table at that hash code location
* Deletion is easy
	* similar to linked list, if we have seperate chaining
		* find the element and then exchange a couple of pointers, finally `free()` the bucket
	* if we don't have seperate chaining, it's easier
		* just hash the data and then go to that location and just delete it
* Loockup is on average better than with linked lists
	* instead of looking up a massive linked lists of size *n*, we have splitted it into *n* number of linked lists and each one just is one-nth the size
	* it's still linear search, just the lenght of the list we search is much much shorter
* Don't do sorting
	* just use an array
* Size is hard to tell
	* hash table with many elements has shorter linked lists, although it may waste many spaces
	* contrast being the smaller the hash table gets, the longer its linked lists get
	* generally it would be bigger than a linked list storing the same data, but smaller than a trie

**Tries**
* Insertion is complex
	* a lot of dynamic memory allocation, especially at the beginning as we build the structure
	* but it's constant time
* Deletion is easy
	* just navigate down a couple of pointers and `free()` the node
	* again, it's constant time
* Lookup is fast
	* it's only based on the length of the data, not the size of the data strucutre
	* if we want to find a 5 characters string in a trie, it just need 5 steps
	* again, it's constant time
* Sorting already done
	* it's kind of sorted as we build the trie
	* or we can say it doesn't need to be sorted
* Rapidly becomes huge
	* if we are storing digits, every node can go to 0, 1, 2, ..., 9, so every node contains information about the data we want to store at that node, plus 10 pointers
		* it would be 80 bytes typically, for every node that we create
	* if we are storing letters instead of digits, that is A, B, C, ..., Z, so every node need 26 pointers
		* it would be 208 bytes for every node
		* then if we have capital case and lower-case, it would be even bigger
___
