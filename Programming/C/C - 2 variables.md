# Creating a variable

format is like this:
`[type] [name];`

eg.
`int number;`
`char letter;`

To create multiple variables of the same type in one line:
`int height, width;`
`float sqrt2, sqrt3, pi;`

It's good design to only declare a variable **right when we need it**.

Actually in many years ago it's ageneral coding practice in C to declare variables at the beginning of the program. But now we don't do it this way.
___

# Using a variable

we just need to type the *variable name* to access it's value. we don't need the `[type]` part.
if we do so, it's re-declaring the same variable name, and weird thing are gonna happen.

```C
int number;         // declaration
number = 17;        // assignment
char letter;        // declaration
letter = 'H';       // assignment

int number = 17;    // declaration + assignment = initialization
char letter = 'H';  // declaration + assignment = initialization
```

remember to just don't do this:
```C
int number;
int number = 17;
// weird things are gonna happen
```
___
