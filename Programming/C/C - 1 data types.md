the following 5 types are built-in to C.
___
# int
* store integers
* always take up 4 bytes of memory = 32 bits
  so information an int store is limited to 32 bits worth
* ranged from $-2^{31}$ to $0$ to $2^{31}-1$
* give us roughly *2 billions* numbers, for both position and negative values

# unsigned int
* it's not a seperate type of variable
* `unsigned` is a *qualifier*[^1], which can modify some certain types (like `int`, `float`)
* it doubles the positive range of variables of that type
* but it disallows negative values 
* ranged from $0$ to $2^{32}-1$, and $2^{31}$ is the middle point
* give us roughly *4 billions* number, for only positive values

[^1]: there are other qualifiers, like `short`, `long`, `const`, etc.
___
# char
* store a single character
* always take up 1 byte of memory = 8 bits
  so information an int store is limited to 8 bits worth
* that means ranged from $-2^7$ to $0$ to $2^7-1$ (if it's represented as number)
  = -128 to 0 to 127
* [**ASCII**](https://www.asciitable.com/) maps characters to numeric values in the positive side of it's range (0 - 127)
```C
'A' - 65
'a' - 97
'0' - 48
'1' - 49
'.' - 46
...
// notice we even need ASCII value for integer
// an int type 1 and a char type '1' look different in memory
int 1    - 00000001 =  1 in decimal
char '1' - 00110001 = 49 in decimal
```
they are just assigned by ASCII. we now just live with them.
___
# float
* store floating-point values, as known as *real numbers*
* always take up 4 bytes of memory = 32 bits, just like `int`
* we can't simply represent the range of `float`
* it has 32 bits of precision, some of which might be used for the integer part, some are for the decimal part.
___
# double
* store floating-point values, just like `float`
* always take up 8 bytes of memory = 64 bits
  = *double precision* than `float`
* so it's much more precise than `float`
___
# void
* it's a *type*, but not *data type*
* we can't create a variable of type `void` and assign a value to it.
* Functions can have `void` return type, which means it don't return a value.
* Parameter list of a function can also be `void`, which means the function takes no parameters.
* more or less, it kind of means *nothing*. it's not doing anything.
* the fact though is more complex. but we don't need to understand that for now.
___
___



the following 2 types are provided by `<cs50.h>` library.
so remember to pound including it first before using those types.
___
# bool
* store a *Boolean*[^2] value
  [^2]: Boolean can only store one of two values: `true` and `false`
* although in many languages `bool` is built-in, but in C surprisingly it's not.
___
# string
* store a series of characters, which programmers typically call a *string*
* = words, sentences, paragraphs, etc.
* it either takes up 4 bytes or 8 bytes in memory
	* if it's a *32-bit machine*, that means every address in memory is 32-bit long, a string is 4 bytes long. if it's a *64-bit machine*, a string is 8 bytes long.
	* see [[CS50x 4-1 Memory#Size of a pointer|Size of a pointer]]
___
___



# custom data type
see more explaination in [[CS50x 3-3 Data Structure#Custom Data Structure|custom data structure]]

we can use `typedef` to rename a data type
`typedef <old name> <new name>;`
* `typedef unsigned char byte;`
* `typedef char* string;`
   this does get used in `<cs50.h>` library

we can also use it to define a custome data type.
eg.
```c
struct car
{
	int year;
	char model[10];
	char plate[7];
	int odometer;
	double engine_size;
};
typedef struct car car_t;
```
or
```c
typedef struct car
{
	int year;
	char model[10];
	char plate[7];
	int odometer;
	double engine_size;
}
car_t;
```
both are the same thing.

later we can use `car_t` like so:
```c
// variable declaration
struct car mycar;   // or
car_t mycar;        // this is the same as the above line

// field accessing
mycar.year = 2011;
mycar.plate = "CS50";
mycar.odometer = 50505;
```
___
___
