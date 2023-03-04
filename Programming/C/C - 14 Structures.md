# Structures

* provide a way to unify several variables of different types into a single, new variable type witn its own type name
* we can group together elements that have a logical connection to one another
	* eg. we can group together a struct `student`, that contains an int `ID number`, a string `name`, and a float `GPA`
* it's like a *super-variable*, a variable that has other variables within it
* recall from [[C - 1 data types#custom data type|custom data type]]
___

# define a structure

for example this is how we may represent a car in C using struct:
```c
struct car
{
	int year;
	char model[10];
	char plate[7];
	int odometer;
	double engine_size;
};
```

we usually define a strucutre in a separate .h files or atop our programs outside of any functions
if we put it into a .h file then we can reuse it in multiple different contexts using `#include`

now we have created a new type
then we can create variables of that type, samw as how we create variables of any other types
eg. `struct car mycar;`
___

# access the fields
we access the various *fields* (also known as *members*) of the structure using the dot operator `.`

```c
// variable declaration
struct car mycar;

// field accessing
mycar.year = 2011;
strcpy(mycar.plate, "CS50");
mycar.odometer = 50505;
```
___

# structures from the heap

like all other data types, we don't have to create structures on the stack, by `struct car mycar;`
we can dynamically allocate structures if we don't know at the beginning of our program how many variables of this type that we're going to need, using pointers and `malloc()`

in order to access the fields of the structures in this situation, we can't just use the dot operator `.` because we need to first [[C - 10 Pointers#Dereferencing|deference]] the pointer to the structure, then we can access its fields

```c
// variable declaration
struct car *mycar = malloc(sizeof(struct car));
// sizeof() actually not just happen to know the size of an int, float, char, etc.
// it actually will compute the size of a data type

// field accessing
(*mycar).year = 2011;
strcpy((*mycar).plate, "CS50");
(*mycar).odometer = 50505;
```

but all those extra parentheses `()`, the star `*`, the dot `.`, are crumblesome right?
surely there's got to be an easier way, it's the *arrow operator* `->`
it does 2 things back-to-back:
1. it dereferences the opinter on the left side of the operator
2. it accesses the field on the right side of the operator

this is how we could improve the above code using the *arrow operator* `->`:
```c
mycar->year = 2011;
strcpy(mycar->plate, "CS50");
mycar->odometer = 50505;
```
___
