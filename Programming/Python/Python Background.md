# Background

Python is a very commonly-used modern programming language
* C was released in 1972
* Python in 1991

Python have been the top 5 or less popular language for around 25 years
* excellent for making complex C operations much simpler
	* String manipulation
	* Networking
	* Data science
		* processing large sets of data
		* generate graphs, charts, and results from them
* **write like English**

Inspired by C
* similar syntax, but easier
* its primary interpreter, *Cpython*, is actually written in C
___

# Writing Python

* To start writing Python, simply open up a file with the .py file extension
	* in CS50 IDE, that will automatically syntax highlight the text inside

* Python is not a *compiled language*
	* Python programs can be run in a Python interpreter
	* similar to PHP
	* the interpreter will run / execute the lines of code, one by one

* CS50 teaches **Python 3**
	* Python 2 is still fairly popular though
___

# Executing Python program

Not only we can pre-writing a .py file,
we can also write and test Python snippets using the Python interpreter from the command line
1. Enter `python` in terminal
2. It will open a Python interpreter, where we can write code
* require to install on the system the Python interpreter though

Generally we still gonna pre-write our .py file though
and then execute it by entering `python <file>` in terminal
* the interpreter will open up the file
* then it will read, run, proceed one line at a time, top to bottom

Lastly we can also run the .py file like how we would run a C program after compiling
1. put the line `#!/usr/bin/env python3` at the very top of the Python file
	* this is a *shebang*, which can automatically find and execute an interpreter
	* also this allow us to execute the .py file as a program
2. but we need to change the permission on the .py file first
	* by entering `chmod a+x <file>` in the CLI
3. then we can execute the .py file by entering `./<file>`
___
