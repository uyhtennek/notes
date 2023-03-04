# Code Principles

1. correctness
2. design
3. style = consistence

easy to read. easy to taught and remember and understand.
___
# Text Editor
= Integrated Development Environment, IDE
eg. Visual Studio Code
(VS Code actually has a 'cloud version')

> a tool that programmers use to write code

A good text editor for programming always highlight the syntax for us based on the programming language that we're using.

If it's a IDE, it'll come with a terminal or [[CS50x 1-1 Programming Environment#Command Line Interface|CLI]] for us to compile code.
almost always come with a nice GUI, a Graphical User Interface.
___
# Compiler

Okay now we wrote a code, `hello.c` for instance. But computer don't understand source code. It only understand binary, 0 and 1. How the source code relate to computer and essentially how the computer runs the source code?

0 and 1 actually can somehow be used not only to represent numbers, letters, colors, audio, video, etc.
but also represent intructions to a computer, like print a messge on screen, or play a sound, or delete a file, etc.

So we have our source code. We need to convert it to binary for computer to run. How?
We can do this by hand. But luckily there is a thing in somewhere in our computer called compiler, which does exactly translating source code to binary.

source code --> `compiler` --> machine code
___
# Command Line Interface
= CLI
often times it means the terminal of Linux
___
# Run `hello.c`

Firstly we need to compile it:
	type `make hello` in CLI

can't see any output or any thing happen.
> it's a good thing to see nothing in programming.
> because it means nothing went wrong, which means everything went right.

`make hello`
it means make a file called `hello`, in current directory
if there is already a `hello` file, it'll get replaced or updated.

`./hello`
run the file `hello`
___
# More Linux commands

`rm hello`
remove file `hello`

`ls`
list all files

`./hello.c`
notice that `hello.c` is not an executable. so this command will give us an error, which is `Permission denied`
It doesn't mean we don't have access to the file. It means the file is not executable. It's not something we have permission to run, but we do have permission to read and write it.
___
# `make` command

Notice `make` is a command, not a compiler.
even though it helps compiling our `hello.c` to a executable file `hello`.

It just knows where to find and use the compiler and it also know to automatically find the `hello.c` file.
___
