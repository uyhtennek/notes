# Scope
* describe from which functions that a variable can be accessed
* **Local variables** can only be accessed within the functions in which they are created
	* for this reason, we **can't** return a local variable created from a function
	* unless it's a pass-by-value type, then it'll return a copy of the returned data
* **Global variables** can be accessed by any function in the program

eg. **local variable**
```c
int main(void)
{
	int result = triple(5);
}

int triple(int x)
{
	return x * 3;
}
```
in this case
* `x` is **local** to the function `triple()`
* `result` is **local** to `main()`

eg. **global variable**
```c
float global = 0.5050;

int main(void)
{
	triple();
	printf("%f\n", global);
}

void triple(void)
{
	global *= 3;
}
```
`global`
* declared outside of all functions
* any function can refer to it
* although it's very flexible, but it could have some dangerous consequences like one function changes the value of that variable before we expect it to be changed.
___

# pass by value

^6c81a1

local variables in C are **passed by value**
* the *callee* ( = the function which call this function) actually doesn't receive the variable itself, but its value.
* it makes a **copy** of the passed variable to work with
* the variable in the *caller* is unchanged

```c
int main(void)
{
	int foo = 4;
	triple(foo);       // after executed triple(foo), foo is still 4
	foo = triple(foo); // foo get overwritten, so it'll be 12 this time
}

int triple (int x)
{
	return x *= 3;
}
```
___

# variable name
* be careful with variable scope if there are multiple variables having the same name
	* that is okay though if all variables of the same name are local, or exist in different scopes

```c
int increment(int x);

int main(void)
{
	int x = 1;
	int y;
	y = increment(x);
	printf("x is %i, y is %i\n", x, y);
}

int increment(int x)
{
	x++;
	return x;
}

// result
x is 1, y is 2
```
___
