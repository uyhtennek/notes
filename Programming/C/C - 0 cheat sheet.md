# types

| data type | size |
|---|---|
| bool | 1 byte |
| char | 1 byte |
| double | 8 bytes |
| float | 4 bytes |
| int | 4 bytes |
| long | 8 bytes |
| string = char* | 4 or 8 bytes |
| pointer | 4 or 8 bytes |
| ... | |
sizes listed here is just how typically a system used for the data type nowadays.
some systems (or libraries?) may use a different size.

---
# CS50 library
* get_char
* get_double
* get_float
* get_int
* get_long
* get_string
* ...
---
# format codes
* `%c` - char
* `%f` - float, double
	`%.2f` - show the number to 2 decimal places, same for doing any number
* `%i` - int
* `%li` - long int
* `%s` - string (or char array)
	* `%s` is special because it can format array, it's designed to print the char until it sees `\0`
* `%p` - pointer
	* it means don't go to the address, just print the address
---
# operators
math
* +
* -
* *
* /
* %

condition
* <
* <=
* >
* >=
* ==

* &&
* ||
* !
---
# syntactic sugar
`counter++;` = `counter += 1;` = `counter = counter + 1;`
`counter--;` = `counter -= 1;` = `counter = counter - 1;`