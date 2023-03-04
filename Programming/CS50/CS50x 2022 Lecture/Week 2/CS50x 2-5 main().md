# main()

usually our code's `main()` function takes no argument:
```c
int main(void)
{
	...
}
```
what `void` here means is just
this program doesn't take any [[Using the Linux Command Line#^89b1fd|command line arguement]]

let look set another `main()`
```c
int main(int argc, string argv[])
{
	...
}
```
C automatically gives **argc** and **argv[]** to us

**argc** stands for argument count
* how many words the user typed at the prompt

**argv[]** stands for argument vector
* an array of all of the words the user typed at the prompt

now we can write `hello.c` without `get_string()`:
```c
int main(int argc, string argv[])
{
	printf("hello, %s\n", argv[0]);
}

// result when execute the program by typing the command
// ./hello Kenneth
hello, ./hello // assume hello is the executable file name
```
so actually we need to use `argv[1]`

but what if we use `argv[2]`?
result now becomes `hello, (null)`

so if we want the program to behave some what more correctly we need to add in some logic to the program to check the command line arguments. something like:
```c
if (argc == 2)
{
	printf("hello, %s\n", argv[1]);
}
else
{
	printf("hello, world");
}
```
___

# why take command line arguments?

it's not necessary but
* it's faster to type
	* since we prompt the user only once
* it get stored in command histroy
	* so we can use the up and down arrow key to quickly get to the executed command
___

# exit status

`main()` also have the ability to signal to the user and the system whether the program executes successfully or not.
and that is by way of its return value. it's why `main()` return an `int`.

take the above `hello.c` as an example. we want to exit the program when the user doesn't input the correct number of arguments. otherwise continue doing its thing.
```c
int main(int argc, string argv[])
{
	if (argc != 2)
	{
		printf("Missing command line argument.");
		return 1;
	}
	// we don't need else here
	// because if the program doesn't exit here
	// it means the user input correctly and argc is 2
	printf("hello, %s\n", argv[1]);
	return 0;
}
```

why use 1?
* any non-zero number indicates to the system something went wrong
* `return 0`, on the other hand, means the program executed well
	* if we have didn't have `return 0` in `main()`, when the program is finished, it assumes everything went well and do `return 0` anyway
* we can use not only 1 to signify errors, but also any number that is not 0
	* sometimes if we executed some faulty program, sometimes it'll have a pop-up with an error message, which contains a *error code* 123 or -49, or etc.
	* then if we call customer support or submit a ticket, we can tell them the *exit status* we encountered (although googling it by ourselves is what more likely to happen)
* actually returning a non-zero number doesn't necessarily means something went wrong. sometimes we do return non-zero number if something went right in a certain way.
	* it's a good practice too.
___
