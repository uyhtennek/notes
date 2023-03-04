# array from the heap

let say we have an array of `[ 1, 2, 3 ]`, now we want to add a 4th number on it.
where can it goes?

surely it goes to the last position, right behind `3` right?
but remember in the context of computers' memory, there could be some stuffs right behind `3` and we surely don't want to overwrite that, because we don't want to lose information.

yet the memoy probably will have room to fit in another array with size of 4 int
so how about just:
1. declare a new array, with size of 4 int
2. copy the old array to the new array, then write `4` as the last element
3. we don't need the old array any more, so let us throw it away

how ever the downside of this approach is obvious already, that is
we need to copy the whole array.
- what if the array is super long?
- what if the array is not a simple int array but an array of interesting data sets?
- this solution is not very optimal if we would like to apply it on web application data sets or mobile app data sets

also what are the [[CS50x 3-1 Algorithms#Big O Notation|big O]] of this?
- To do searching:
   $O(n)$ / $O(n\log{}n)$, $\Omega(1)$
- To do inserting like the above:
   $O(n)$, $\Omega(1)$^1
both are quite slow actually if we have a large enough data set

[^1]: it is $\Omega(1)$ because we can just try putting the `4` in the last position anyway before any operation to see if we get lucky that the computer is not using that byte in memory

But anyway let us implement this feature in code: ^cc121e
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
	// we can't use _int list[3]_, why?
	// we would declare this array has a size of 3 int permanently
	
	int *list = malloc(3 * sizeof(int));
	// this instead lay the foundamental to make us able to resize the array
	
	// also remember that malloc() get memory from heap
	// _int list[3]_ get memory from stack
	
	if (list == NULL)
	{
		return 1;
	}
	
	list[0] = 1;
	list[1] = 2;
	list[2] = 3;
	// notice how we can treat these memory from malloc()
	// just like how we would treat a normal array, using square brack []
	
	//*list = 1;
	//*(list + 1) = 2;
	//*(list + 2) = 3;
	// or we can use pointer arithmetic instead
	// if we want to be a troll
	
	
	// some time later we realize we actually need an array of size 4
	
	int *tmp = malloc(4 * sizeof(int));
	// why can't we just do: list = malloc(4 * sizeof(int));
	// if we do so then numbers from the original list would be orphaned (= lost)
	
	if (tmp == NULL)
	{
		free(list);
		return 1;
	}
	
	for (int i = 0; i < 3; i++)
	{
		tmp[i] = list[i];
	}
	list[3] = 4;

	free(list);
	list = tmp;
	
	for (int i = 0; i < 4; i++)
	{
		printf("%i\n", list[i]);
	}
	
	free(list);
	// why don't we need to free(tmp)?
	// list and tmp, they are pointing to the same location in memory
	// in this case, free(list) or free(tmp) is just the same
	// but still we argue that free(list) makes more sense
	
	return 0;
}
```

^b3ef0e

___

# realloc()
C actually can help us resize an array. It's `realloc()` from `<stdlib.h>`.

Let see how it goes in code.
```C
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
	// Dynamically allocate an array of size 3
	int *list = malloc(3 * sizeof(int));
	if (list == NULL)
	{
		free(list);
		return 1;
	}
	
	// Assign three numbers to that array
	list[0] = 1;
	list[1] = 2;
	list[2] = 3;
	
	// Time passes
	
	// Resize of old array to be of size 4
	int *tmp = realloc(list, 4 * sizeof(int));
	if (tmp == NULL)
	{
		free(list);
		return 1;
	}
	
	// Add fourth number to new array
	list[3] = 4;
	// we skipped the whole tmp part
	// because realloc() would do all those for us
	
	// Print new array
	// Free new array
	return 0;
}
```

* It can resize a given array
	1. it looks at the memory address of the end of the given array, to see if there after have room for the new size
	2. it yes, just occupy those spaces
	3. if not, move the whole array to an available place
	4. also copy the information in the old addresses to the new addresses
	5. `free()` the old chunk of memory
	6. return the (first) address of the new chunk of memory
___
