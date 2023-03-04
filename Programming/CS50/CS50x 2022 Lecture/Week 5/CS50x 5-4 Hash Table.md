# Hash table
it's like a *Swiss army knife* of data structures
it combines the random access ability of an array with the dynamism of a [[CS50x 5-2 Linked Lists|linked list]]

it allows us to associate *keys* with *value*
* it's very commonly used
* eg. associate a username with a password, or a name with a number, etc.
* used basically when ever we would like to give something as input, and get back as output a corresponding piece of informations
* contains 2 things
	* a [[#^b4953f|hash function]]
	* an array of the type we wish to place into

the idea is that we run our data through the hash function, that give us a hash code, then we store the data in the element of the array with that hash code

how it looks like, is just pretty much like an array:
| index | hash|
| :-: | :-: |
| 0 | |
| 1 | |
| 2 | |
| 3 | |
| 4 | |
| ... | |

much like an array, a hash table allow us to jump to any location randomly and instantly
like if we want to store names of people and say we have a hash table of 26 *boxes*[^1],
wouldn't it be nice if the person's name starts with A, we just put it into the first box, and if it starts with Z, we put it at the bottom
and these operations are instant since they just go exactly to the location that they need to go ^ecb8c1

[^1]: boxes from a hash table, are genearlly called *buckets*, which we can put values

eg.
| starts with | bucket | points to bucket |
| :-: | --- | --- |
| A | | -> Albus |
| B | | |
| C | | -> Cedric
| D | | -> Draco |
| E | | |
| ... | | |
![[hash-table-1.png|200]]
but eventually we would have people names' start with A or C or D, again. so where do these new names go?

notice for each bucket, they are not pointing to names only, but the bucket, then for each bucket that have a name, we can have it points to the another bucket, that could have a name in it
| starts with | bucket | points to bucket | points to bucket |
| :-: | --- | --- | --- |
| A | | -> Albus | |
| B | | | |
| C | | -> Cedric | -> Celeste |
| D | | -> Draco | |
| E | | | |
| ... | | | |
![[hash-table-2.png|500]]

so a **hash table** is bascially an **array**, whose elements are [[CS50x 5-2 Linked Lists|linked lists]]
this in theory give us the best of both world actually
* we can jump immediately to the location we want to
* but if that location is already occupied, it devolves into a linked list
	* but we wouldn't need to start from first node of one massive linked list
	* we start from the first node of the list that start with A or B or C, etc.
* the running time is better than $O(n)$ at least

how we represent something like this in code?
```c
typedef struct node
{
	char word[LONGEST_WORD + 1];
	// `LONGEST_WORD` is just defined by us, set it to what ever length we need
	// remember we need `+1` to make room for the NUL character
	struct node *next;
}
node;
```

then here is how we would declare a hash table.
```c
int main(void)
{
	node *hash_table[NUMBER_OF_BUCKETS];
}
```

next, how do we put something in a hash table?
we're gonna need something called a *hash function* ^b4953f
* it takes an input, maybe it's an int, a string, a char, etc., then produces as output a location, an non-negative integer value called a *hash code*
* according to the above story, if we input `Albus`, it returns `0`, so we can put `Albus` in bucket 0. if we input `Zacharias`, it returns `25`
	* the hash function here just look at the first char from the input string, then doing some math to get back in number between 0 and 25
___

# Limitation of hash tables

continuing the above story, what if names that start with the letter H is very popular in our story
it would make the list of bucket H very long
how may we solve this problem?

perhaps instead of just have a bucket storing names that start with the letter H, we can have some more buckets storing names that start with Ha, Hb, Hc, etc., and a hash function that look at not just the first letter but also the second letter of the names

however, if we now look at not just A through Z, but AA through ZZ, how many buckets that are?
= $26^2$ = 676 buckets
how about AAA through ZZZ?
= 17,576 buckets

the upside is that we get to do the search in constant time =$O(3)$, if every bucket contains 1 name only
the downside is that we now have up to 17,576 buckets, which is not a big deal since computers these days have plenty of memory, but how often a name would starts with Heq, Azz, Bhp, Zzz, Aaa, etc.
a lot of not useful combinations or empty buckets are there, using some of the memory, just because we need them to be there to do the math and jump to the precise location
___

# Running time of hash tables

**Search / Insert**
* $O(n)$.
	* the worst case would be every one in the story has a name starts with the same letter.
	* it would make the chain very long. at that point, it's just a linked list.
* but if we have a better practice, the running time, again using the above example, should be 26 times faster than a linked list alone

To **search** a data we just need to run the input through the hash function, and then we can go directly to the hash code location to see if the data is there

**Insertion, Deletion, Lookup** can start to get close to **constant** time, toward $\theta(1)$
but for **Sorting** though, hash tables are pretty bad. if we have to sort it basically regresses to a linked list. Running time in **insertion** and **deletion** become closer to $\theta(n)$.
so only use hash tables when we don't care about the order.
___

# hash function

there is no limit how we implement our hash functions.
but there are some guidelines to write a good one:
1. Use only the data being hashed and all of the data being hashed
    it means the data is the only input, and we should use all information from that data to determine where it goes, but not just using a part of its information
2. Be deterministic ( 確定性的 )
    every time we pass the same data into the hash function, it always give the same hashcode
3. Uniformly distribute data
    we should spread out the data throughout the table to utilize every element in the array
4. Generate very different hash codes for very similar data
    so that similar data would not be concentrated in the same element in the array

here is an example of a hash function
```c
unsigned int hash(char* str)
{
	int sum = 0;
	for (int j = 0; str[j] != '\0'; j++)
	{
		sum += str[j];
	}
	return sum % HASH_MAX;
}
```

but generally we wouldn't write our own hash functions
since there are many good hash functions in the Internet already.
it's an unnecessary waste of time to create a hash function of our own.
but remember to cite the sources though, for the people contributing to the community.
___

# collisions
a collision occurs when 2 pieces of data, when run through the hash function, yield the same hash code

we can't just overwrite the data that placed in there first, so how can we find a way to get both elements into the hash table, while trying to preserve quick insertion and lookup?


**Linear probing**
so we can't put the second data at the hash code location as it's occupied, then how about the hash code plus 1 location? if that is not okay too then let's put that in hash code plus 2.

later if we want to find that data, we run it through the hash function but can't find it in the hash code location, we can go looking for hash code plus 1 or plus 2 location, and hopefully the data is somewhere nearby the hash code location.

however, as we have more data that has the same hash code, they will get further away from the hash code location. plus, since that hash code plus 1 location is occupied too, we now have double the chance that we're going to have another collision, the *cluster* grows by 1
this problem is known as *clustering*

then the running time that used to be $O(1)$ is a little bit closer to $\theta(n)$ now
because the likelihood of having a collision keep growing and growing
eventually it's just as bad as not sorting the data at all (= not using the hash function)

another limitation for any probing technique is that we're limited to storing as much data as we have locations in our array
say we have an array of size 10, we want to insert the 11th or 12th element, it seems we will get stuck in an infinite loop trying to find an empty spot


**Chaining**
instead of each element of the array holding just one piece of data, it held multiple pieces of data. it's made possible by making each element of the array is a pointer pointing to the head of a linked list.
then multiple pieces of data can yield the same hash code, and we'll be able to store them all.

* *clustering* are eliminated
* insertion for linked list is an $O(1)$ operation
* for lookup we only need to search through a hopefully small list
   since we're distributing one huge list across *n* lists

how about when *collisions* happen?
we can just insert the new data into the linked list pointed by the pointer at the hash code location, also remember we insert it to the beginning so it's immediate
___
