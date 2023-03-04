# Queue
it's a data structure much like a line outside of a store
* it has a *FIFO* property
	* FIFO: First In, First Out
	* great for fairness, at least in our human world
* another example would be *printer queues*
	* the first person to send their essay to the printer should ideally be printed before the last person to send their essay to the printer

how do we implement a queue?
we need to implement 2 fundamental operations
* *enqueue*
   adding something to the queue
* *dequeue*
   removing something from the queue

using *printer queue* for instance, if we send a bunch of documents to the printer, it would need a chunk of memory to store them first, but in what form?

how about using an array?
the problem is an array colud be filled. at some point the printer will run out of memory and people need to wait until the printer printed all the stuffs and get an new empty array to use.

so perhaps we should dynamically resizes the array, even use a linked list
___

# Stack
* it's like the cafeteria trays that stack up on top of each other
	* when we pick up a tray, the tray is the last tray that was cleaned, but not the first tray that was cleaned
* it has a *LIFO* property
	* LIFO: Last In, First Out
* like how *Gmail* inbox sorts emails by default, the newest email, last in, is the first one at the top of the inbox

what are the 2 fundamental operations?
* *push*
   push something onto the top of a stack
* *pop*
   pop something out of the top of a stack
___

# Dictionary
it's an abstract data type that contains *key-value pairs*
* Arrays, [[CS50x 5-4 Hash Table|Hash table]], [[CS50x 5-5 tries|Tries]] are example of a dictionary
	* Arrays
		* key is th element index
		* value is the data at that location
	* Hash tables
		* key is the hash code
		* value is a linked list of data hashing to that hash code
	* Tries
		* key is guaranteed to be unique
		* value could be a Boolean that tells us whether the data exists in the structure
* it has *keys* and *values*
	* like in human world's dictionary, has words and their definitions
	* *sweetgreen* (a salad restaurant) has letters of the alphabet as keys and salads as their values
* the problem of any dictionary is quite the same as the [[CS50x 5-4 Hash Table#Running time of hash tables|problem with Hash table]]
	* which is it would not perform well when a key contains many values
___
