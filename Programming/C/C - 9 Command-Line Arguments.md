# main()
see [[CS50x 2-5 main()]]

usually we use `int main(void)` to begin our program
but that is because we are not going to collect input through command-line
maybe we don't need it, or maybe we instead collect user input through in-program prompts

what if we do want to collect input through command-line?
to collect *command-line arguments*, we need to declare `main()` as
`int main(int argc, string argv[])`

* **argc**
	* stands for *argument count*
	* an int
	* stores the number of command-line arguments the user provided
| command | argc |
| --- | :-: |
| `./greedy` | 1 |
| `./greedy 1024 cs50` | 3 |

* **argv**
	* stands for *arguement vector*
	* an array of string
		* one string per element
		* also the last element of `argv` is always at `argv[argc-1]`
	* stores the actual text the user typed at the command-line
* for example when we execute `./greedy 1024 cs50`
| argv indices | argv contents |
| :-: | :-: |
| argv[0] | ".greedy/" |
| argv[1] | "1024" |
| argv[2] | "cs50" |
| argv[3] | ??? |
since `argv` is a string array, `argv[1]` is not integer 1024, but string "1024", as explained in [[C - 1 data types#char|data type - char]]
___
