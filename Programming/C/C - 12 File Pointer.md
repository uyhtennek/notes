# `FILE`
we can have program to *read data from* and *write data to* files, which allows us to store *persistent data*, that won't disappear when our program stops running

C provides a data structure known as `FILE`
and we usually working with files by using pointers to them, `FILE*`

all related functions live in `<stdio.h>`
every file input/output (I/O) functions accept `FILE*` as one of the parameters, except `fopen()`
since it is used to get a file pointer
___

# basic `FILE*` functions

`fopen()`
* opens a file and returns a file pointer to it
* always check for `NULL`, see [[C - 10 Pointers#Dereferencing|dereference]]
* `FILE *ptr = fopen(<filename>, <operation>)`
* `FILE *ptr1 = fopen("file1.txt", "r")`
	* `r` stands for *read*
* `FILE *ptr2 = fopen("file2.txt", "w")`
	* `w` stands for *write*
	* it's going to write data start at the very beginning = overwrite `file2.txt`, deleting it's information if that's already there
* `FILE *ptr3 = fopen("file3.txt", "a")`
	* `a` stands for *append*
	* it starts writing from the end of the file
* if we want to open a file for both `r` and `w`, we need to open 2 separate file pointers to the same file but with different operations

`fclose()`
* closes the file pointed to by the given file pointer
* `fclose(<file pointer>);`
* `fclose(ptr1);`


`fgetc()`
* read the **next** character, and store it in a char variable
* when we do `fopen()` for this file, the operation must be `r` for read
	* otherwise we will suffer an error
* `char ch = fgetc(<file pointer>);`
* `char ch = fgetc(ptr1);`
```c
// to read all the characters from a file and print them to the screen
char ch;
while((ch = fgetc(ptr)) != EOF)
	printf("%c", ch);

// EOF = end of file, a special character defined in <stdio.h>
// a Linux command "cat", essentially does just this
```

`fputc()`
* writes or appends a character to the pointed-to file
* much like `fgetc()`, `fputc()` requires a file opend with operation `w` or `a`
* `fputc('A', ptr2);`
```c
// to copy all the characters from a file to another file
char ch;
while((ch = fgetc(ptr)) != EOF)
	fputc(ch, ptr2);

// a Linux command "cp", does just this
```


`fread()`
* it's like a improved version of `fgetc()`, that allows us to get any amount of information
	* `fgetc()` only get the next character at a time
	* `fread()` can get the next 100 or 500 or more bytes at a time
* `fread(<buffer>, <size>, <qty>, <file pointer>);`
	* `<buffer>`: a **pointer** points to where we store the information read from the file
	* `<size>`: how large each unit of information will be
	* `<qty>`: stands for *quantity*, how many units of information that we are going to read
	* `<file pointer>`: a pointer that points to the file that we will read from
```c
int arr[10];
fread(arr, sizeof(int), 10, ptr);
// 1. make an int array of size 10
//    a 40 bytes memory from stack is set here
// 2. read 10 int, or 4-byte large data, from a file pointed by ptr
// 3. store 40 bytes data it read to arr

// similarily we can do this
double *arr2 = malloc(sizeof(double) * 80);
fread(arr2, sizeof(double), 80, ptr);
// this time arr2 is dynamically allocated,
// so the memory it uses is from heap
// but we can still store the information we read from the file in it

// lastly this is the same as fgetc()
char c;
fread(&c, sizeof(char), 1, ptr);
```

`fwrite()`
* just opposite to `fread()`, it writes instead of reads. it takes information from `<buffer>` and put that into the file pointed by `<file pointer>`
* `fwrite(<buffer>, <size>, <qty>, <file pointer>);`
```c
// the following examples are much like what are in fread()

int arr[10];
fwrite(arr, sizeof(int), 10, ptr);

double *arr2 = malloc(sizeof(double) * 80);
fwirte(arr2, sizeof(double), 80, ptr);

char c;
fwrite(&c, sizeof(char), 1, ptr);
```


some more useful file pointer functions are:
| Function | Description |
| --- | --- |
| `fgets()` | Reads a full string from a file. |
| `fputs()` | Writes a full string to a file. |
| `fprintf()` | Writes a formatted string to a file. |
| `fseek()` | Allows you rewind or fast-forward within a file. |
| `ftell()` | Tells you at what (byte) position you are at within a file. |
| `feof()` | Tells you whether you've read to the end of a file. |
| `ferror()` | Indicates whether an error has occurred in working with a file. |
___
