# Limitation of Array

let say we want to develop a phone book app.
we need to include names and phone numbers in it. so we made 2 arrays:
```c
string names[] = { "Carter", "David" };
string numbers[] = { "+1-617-495-1000", "+1-949-468-2750" }

// we use string for phone number here because
// 1. it supports symbols like + or - that are commonly found in a phone number
// 2. leading zero means nothing as int, eg. 2138 vs 02138
//    2138 and 02138 are actually ZIP code, then it leads to 2 different addresses
```
is it well-designed?

Imagine the next step is we need to find David:
```c
for (int i = 0; i < 2; i++)
{
	if (strcmp(names[i], "David") == 0)
	{
		printf("Found %s\n", numbers[i]);
		return 0;
	}
}
printf("Not found\n");
return 1;
```

It's easy to make mistake and it's vulnerable to mistakes.
* it allows us to make `names` and `numbers` to have different sizes.
* it's trusting the programmer to always line up the number for accessing `names` and `numbers`. that is the number of `i` in `names[i]` and `numbers[i]`.
	* but that's not going to happen when the program is getting much much longer and the logic is getting much sophisticated.

We need to couple the 2 informations in a way that.
* it doesn't just trusting the 2 informations have tight relationship with themselves.
* we access the 2 informations using the same variable

= we combine `names` and `numbers` instead of keeping them seperated
but how?
___

# Custom Data Structure
> in C, we have the ability to invent our own data types, that go beyond the built in ints, floats, bools, strings, etc.

eg.
we can create a `person` data type, that contains 2 strings `name` and `value`
```c
typedef struct
{
	string name;
	string number;
}
person;
```
also it's needed to put atop of `main()` because [[CS50x 2-1 Compiling#clang|clang]] read code from top to bottom.

`typedef` means *define a new data type*
it's a C key word that let us create our own data type

`struct` means *structure*
it tells the compiler that this data type is just a simple data type, just like an int or a float, but containing some primitive data type and then renamed `person`

to summary, the compilier [[CS50x 2-1 Compiling#clang|clang]] will know there is a new data type named `person`, and it's composed of a string `name` and another string `number`
`name` and `number` are *encapsulated* = contained inside of the same data type `person`

and then from now on,
instead of using 2 seperate arrays `names` and `numbers`,
we can just use an array of `person`.

eg.
```c
person people[2];

people[0].name = "Carter";
people[0].number = "+1-617-495-1000";

people[1].name = "David";
people[1].number = "+1-949-468-2750";

for (int i = 0; i < 2; i++)
{
	if (strcmp(people[i].name, "David") == 0)
	{
		printf("Found %s\n", people[i].number);
		return 0;
	}
}
printf("Not found\n");
return 1;
```


**more real-life example**

* What is an Image?
	* a bunch of pixels or dots on the screen
		* each dot has a RGB value
			* Red value
			* Green value
			* Blue value
```c
typedef struct
{
	int red;
	int blue;
	int green;
}
pixel;

pixel image[1000];
```

* Music / Sound
	* musical note
	* duration
	* loudness

* big int
   in some financial or scientific software, either int and long are not enough for calculating their things, so we have a structure called *big int* in some languages to deal with the [[CS50x 1-3 Data types limitation & Programming mindsets#bits limit overflow|integer overflow problem]]
   * it uses an array of values
   * based on how high we want to count, it stores more and more bits


**side notes: Object**

C is not a object-oriented language. Java, C++, Python, etc. are.
In those object-oriented language they have things called *classes* or *objects*.

*object*: store not just data ( = variables ), but also functions

in C, we only have *data structures* that store data
___
