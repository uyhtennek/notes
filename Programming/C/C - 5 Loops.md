# Loops

^6476f8

* allow our programs to execute lines of code repeatly
   so that we don't need to repeat lines of code
___

# while (true)
```C
while (true)
{
	// some code
	// which will run forever
}
```
this will run forever. because `true` will always be *true*.
it's an *infinite loop*.

unless, until we break out of it, like with a `break;` statement
or otherwise kill our program[^1]

[^1]: 'Ctrl + C' is how to kill a program in Linux CLI
___

# while (boolean-expr)
```C
while (boolean-expr)
{
	// some code
	// which will run until boolean-expr evaluates to false
}
```
___

# do-while
```C
do
{
	// some code
	// which will run once
	// and then repeat when ever boolean-expr is true
	// until boolean-expr is false
}
while (boolean-expr);
```
___

# for loops
```C
for (int i = 0; i < 10; i++)
{
	// some code
	// which will repeat until i is 10
	// so in this case it will run 10 times
}
```
used to repeat the body of a loop a *specified number of times*

What happen in that one complex line?
1. Before the loop starts, run `int i = 0;`
2. Evaluate **immediately** the Boolean expression `i < 10`
    `i` is 0 now right? so it is true. and then the loop run.
    but if `i` is 15 here. it is false, then the loop will not run.
3. So if the loop get run, after 1 loop, `i++` will run 1 time
4. After that, the Boolean expression `i < 10` is checked again
    if true, run the loop one more time (= repeat 3)
    if false, the loop ends

and in that one complex line,
as long as 2 semi-colons are put inside the braces, it can be anything
```C
for (start; expr; increment)
{

}
```
___

# Comparison

|  | repeat how many times | run at least once | common use case |
|---|:---:|:---:|---|
| while | unknown | no | running control flow for a game \^[^2] |
| do-while | unknown | yes | prompting user for input \^[^3] |
| for | usually somehow defined | no | run the loop a number of times based on what the user input \^[^4]|

[^2]: say we want to do something as long as the game is not over, but since we don't know when the game is going to over, `while` loop is useful

[^3]: we want to prompt the user for input at least once, but we don't know if the user is going to input something valid or useful, if not then we prompt again

[^4]: in the moment we compile the code we don't know how many times the loop should run, since it is based on user's input

however in many cases we can interchange them.
___
