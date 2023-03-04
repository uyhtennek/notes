
# Conditionals
> Conditional expressions allows our programs to make decisions and take different forks in the road, also known as branches

**cheat sheet**
* if / else-if
* switch
* ? :

in a boolean-expression,
if its value is `0`, it is `false`
if its value is **not** `0`, it is `true`
___

# if / else-if
```C
if (boolean-expression)
{
  // some code
  // which will run only if the 'boolean-expression' evaluates to true
  // if false, it get skipped
}
-------------------------------------
if (boolean-expression)
{
  // some code
  // which will run only if the 'boolean-expression' evaluates to true
  // same as above
}
else
{
  // some code
  // which will run only if the 'boolean-expression' evaluates to false
}
-------------------------------------
if (boolean-expr1)
{
  // first branch
}
else if (boolean-expr2)
{
  // second branch
}
else if (boolean-expr3)
{
  // third branch
}
else
{
  // fourth branch
}
// those 4 branches are mutually exclusive
// if boolean-expr1 is true, first branch gets run, none other will get run
// if boolean-expr2 is true, second branch gets run, none other will get run
// so forth
-------------------------------------
if (boolean-expr1)
{
  // first branch
}
if (boolean-expr2)
{
  // second branch
}
if (boolean-expr3)
{
  // third branch
}
else // else binds to the nearest if ONLY
{
  // fourth branch
}
// in this case only the third and forth branches are mutually exclusive
// if the value satisfy all 3 expressions,
// first, second, third branches will run
-------------------------------------
```
___

# switch
```C
int x = GetInt();
switch(x)
{
    case 1:
	    printf("One!\n");
	    break;
	case 2:
	    printf("Two!\n");
	    break;
	case 3:
	    printf("Three!\n");
	    break;
	default:
	    printf("Sorry!\n");
	    break;
}
```
it makes decisions by **cases** rather than relying on Boolean expressions

`break` is important to be there, otherwise it will "fall through" each case.
in the above case if `x = 1`, the printed result would be:
> One!
> Two!
> Three!
> Sorry!

but sometimes we do want it to do so, like this:
```C
int x = GetInt();
switch(x)
{
	case 5:
		printf("Five, ");
	case 4:
		printf("Four, ");
	case 3:
		printf("Three, ");
	case 2:
		printf("Two, ");
	case 1:
		printf("One, ");
	default:
		printf("Blast-off!\n");
}
```
___

# Ternary operator `? :`
```C
int x;
if (expr)
{
    x = 5;
}
else
{
    x = 6;
}
---------------
int x = (expr) ? 5 : 6;
```
the 2 snippets of code act identically.
it's known as **ternary operator**. it's just a cute trick, useful for writing very short conditional branches.
___
