`ls`
* short for *list*
* give a readout of all the files and folders in the current directory
* items are categorized with different color:
```
blue text with a slash /     -->  folder
green text with a asterisk * -->  executable file
default / white / black text -->  file
```


`cd <directory>`
* short for *change directory*
* change the current directory to `<directory>`, which we specify

`.`
shorthand name for the *current directory*

`..`
shorthand name for the *parent directory* of the current directory

`cd ..`
move to the parent directory

`cd ../..`
to move to the parent parent directory

`cd`
to move to the root directory


`pwd`
* short for *present working directory*
* give the name, actually *full path*, of the current directory
* but the terminal prompt will often tell us the name of the current directory


`mkdir <directory>`
* short for *make directory*
* create a new sub-directory called `<directory>`
* located in the current directory


`cp <source> <destination>`
* short for *copy*
* create a duplicate of the file we specify as `<source>`, and save it in `<destination>`
* eg. `cp hello.txt hi.txt`
* what if `<source>` is a folder?
	* `cp` don't know what to do with it. so it does nothing but gives us a message.

`cp -r <source directory> <destination directory>`
* copy entire directory, specified as `<source directory>`
* `-r` stands for *recursive*
	* it tells `cp` to *recursively* dive down into the direcotry and copy everything inside of it
	* including sub-directories


`rm <file>`
* short for *remove*
* it will delete `<file>`, but it will ask us to confirm (y/n) first
* once it's deleted, there is no way to recover it

`rm -f <file>`
* `-f` stands for *force*
* it skip the confirmation

`rm -r <directory>`
* need `-r` to delete the entire directory
* much like the case in `cp -r`
* but it'll ask for confirmation for every item in the directory

`rm -rf`
* just combining `-r` flag and `-f` flag
* `rm -fr` / `rm -r -f` / `rm -f -r`, they are the same as `rm -rf`


`mv <source> <destination>`
* short for *move*
* but it does *rename* a file
	* moving it from `<source>` to `<destination>`
___

`make <name>`
* compile text file to an executable, and give it a name
* underneath it's running `clang` to compile **C** file, which stand for *C language*


`clang <c file>` ^943345
* eg. `clang hello.c`
* `clang` is the compiler, it compiles the `<c file>` and output an executable
* but the compiled file is not named *hello*, instead it's named *a.out*
	* it's the default file name for compiling using `clang`. (the name is a historical convention)
	* stands for *assembler output*

`clang -o <name> <c file>`
* we use this if we want to give the compiled executable a name
* `-o` shorts for *output*
* but it can't compile the `<c file>` if the code used any function that is from libraries
	* because the functions are not defined
	* explaination in [[CS50x 2-1 Compiling]]

`clang -o <name> <c file> -lcs50`
* this can link the library `cs50` to our program
* similarly, to use the `math` library, we need `-lm`. and same for other libraries
___

`chmod`
`ln`
`touch`
`rmdir`
`man`
`diff`
`sudo`
`clear`
`telnet`
