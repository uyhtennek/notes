# Arrays
* a fundamental **data structure**
* hold values of the same type, at *contiguous* memory locations
	* that is to say, a bunch of value of the same type are groupped together in memory very close to each other
	* allow us to not give each one its own variable name
* it's like a *mail bank* and its *post office boxes*
| | Arrays | Post Office Boxes |
| --- | :-: | :-: |
| mother of all is... | array | mail bank |
| have multiple... | elements | post office boxes |
| containing... | data | mail |
| with type... | int or char | letters or small packages |
| accesse by... | index | mailbox number |
* use zero-indexing
	* = count from 0
	* 1st element is located at index 0
	* last element is located at index n-1
* C is lenient ( 寛容 ) enough that it won't prevent us from going "out of bounds" of the array
	* the code can still compile
	* but in run time it's likely to cause problems
___

# declarations
`type name[size];`
* `type` - the kind of variable each element of the array will be
* `name` - name
* `size` - how many elements we want the array to contain
eg.
`int student_grade[40];`
`double menu_prices[8];`

we can declare and initialize an array with starting values:
```c
bool truthtable[3] = { false, true, true };

// also we can do:
bool truthtable[] = { false, true, true };

// they are the same as
bool truthtable[3];
truthtable[0] = false;
truthtable[1] = true;
truthtable[2] = true;

// similarly we can do the initialization with a loop
int arr[100];
for (int i = 0; i < 99; i++)
{
	arr[i] = i;
}
```
___

# using an array
* a single element of an array works the same as any other variable of the same type as the array
   since they effectively are the same data type

* what if we go "out of bounds"?
```c
bool truthtable[10];
truthtable[10] = true;
```
it still can compile. we may even get away with it in run time.
but just it's not the best move. since we don't know what will happen.

* also we can't treat the entire array as a variable
	* we can't assign one array to another array with `=`
	* if we want to do that, we need a loop to copy each element in the array to the another
```c
int foo[5] = { 1, 2, 3, 4, 5 };
int bar[5];

bar = foo; // this will not work

// we need to do this instead
for (int j = 0; j < 5; j++)
{
	bar[j] = foo[j];
}
```
___

# multi dimension array
eg. `bool battleship[10][10]`;

Array can consist of more than a single dimension
* think of it as a 10 x 10 grid of cells
* however in memory it's still a 100-element one-dimensional array
	* same goes to 2-dimension or 3-dimension arrays, etc.
___

# pass by reference

recall that most variables in C are [[C - 7 Variable Scope#pass by value|passed by value]] in function calls
arrays however are **passed by reference**
* the *callee* ( = the function we pass the array to ) receives the actual array, **not** a copy of it
	* it makes sense because arrays can be really large, and it takes so much time and effort to make a copy of a big array
	* not worth it to make a copy and then eventually just discard it
* so the memory where the array is stored at get changed when the *callee* manipulates elements of the array

```c
void set_array(int array[4]);
void set_int(int x);

int main(void)
{
	int a = 10;
	int b[4] = { 0, 1, 2, 3 };
	set_int(a);
	set_array(b);
	printf("%d, %d\n", a, b[0]);
}

void set_array(int array[4])
{
	array[0] = 22;
}

void set_int(int x)
{
	x = 22;
}

// result is ?
10, 22
```
___
