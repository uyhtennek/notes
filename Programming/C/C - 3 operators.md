# Arithmetic Operators

```C
add        ->   +
subtract   ->   -
multiply   ->   *
divide     ->   /

eg.
int x = y + 1;
x = x * 5;

// it's the same as:
x *= 5; // same can do to +=, -+, *=, /=, %=
```
remember if any value is a floating point number, the result will also be a floating point number

and then there is the **modulus operator**
```C
modulus    ->   %

int m = 13 % 4;  // m is now 1
```
it gives us the **remainder**, when the number on the left of the operator is divied by the number on the right.

and modulus is actually quite frequent to use, like maybe in a Random number generator:
1. we want to pick a random number from 0 to 19
2. we take what ever huge number
3. divide it by 20
4. get the remainder

then we also have shorthand for incrementing or decrementing a variable by 1:
(because it's very common to do so)
```C
x++;
x--;
```
___

# Boolean Expressions
* used for comparing values
* can only evaluate to either `true` or `false`
* commonly used in **deciding the code branch to take**, or **determining whether a loop should continue**.
* sometimes it works with type `bool`, but it don't have to
* In C, **every** *non-zero* value is equivalent to `true`
  and *zero* is `false`.
* So `int x = 4;` is equivalent to `bool z = true`.

Operators: ^0e156e
```C
Logical AND --> &&
Logical OR  --> ||  // this symbol | is known as 'vertical bar'
Logical NOT --> !

Less than                -->  x < y
Less than or equal to    -->  x <= y
Greater than             -->  x > y
Greater than or equal to -->  x >= y

Equality                 -->  x == y   // be careful not to use '='
Inequality               -->  x != y
```
| x | y | (x && y) | (x \|\| y) | !x |
|:----:|:----:|:----:|:----:|:----:|
| true | true | true | true | false |
| true | false | false | true | false |
| false | true | false | true | true |
| false | false | false | false | true |
___

# string comparison
in C we can't compare 2 strings with `==`.
eg. `"Ron" == "Ron"`
it will give compiling error.

because `string` is **not** a data type.
`int`, `float`, `bool` are. they are built-in to C language. `string` isn't.

we're actually given a different function to do this
`strcmp()`, known as *string compare*
if the 2 strings are equivalent, case-sensitively, it will return `0`
( yeah it for some reason returns `false`, not very intuitive... )
also it returns a positive number or a negative number, based on how many ASCII value the 1st string comes before or after the 2nd string, so that we can sort the 2 strings.

although we could check for `!strcmp()` instead of `strcmp() == 0`,
but it's arguably a worse design since `strcmp()` is returning an int, using `!` would hide this detail
___
