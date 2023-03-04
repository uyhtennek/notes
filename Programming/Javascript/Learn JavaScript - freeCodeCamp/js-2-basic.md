# Comment
// in-line comment
/* multi-line comment \*/
___
# 7 data types
1. undefined
	* something that hasn't been defined
	* a variable that havn't been set to anything yet
2. null
	* it means nothing
	* a variable that have been set to nothing
3. boolean
4. string
5. symbol
	* an immutable primitive value that is unique
6. number
7. object
	* can store a lot of different key value pairs
---
# Declaring a variable
1. `var`
	* stands for _variable_
	* can set to another data type
	* able to be used throughout our whole program
```javascript
var myName = "Kenneth"
myName = 8
```
2. `let`
	* only be able to used within the scope of where we declared that
```javascript
let ourName = "冇人識你呀"
```
3. `const`
	* a variable that should and can never change
	* will get an ERROR if try to change it
```javascript
const pi = 3.14
```
---
# Assigning a variable
```javascript
var a;     // just declaring a variable
// at this point 'a' is an uninitalized variable

var b = 2; // initializing (declaring and assigning) a variable
// 'var b' is the declare part
// '= 2' is the assign part, where the '=' sign is the assignment operator

a = 7;     // assigning value '7' to variable 'a'
b = a;     // assigning the content of 'a' to 'b'
```
___
# Console.log
It allows us to see things in the console.
`console.log("Hello world!")`
___
# Syntax
* __semicolon ;__
	* It's not necessary in JavaScript. But it's a good practice to place it so the end of the line is clear to see.
* JavaScript is __case sensitive__
	* variables and functions are case sensitive.
	* `camelCase` is not the same as `CamelCase`.
	* it's common practice to use __camel case__
		* first letter is always lower-case
		* if there is a new word or a new section of a word in the following, we capitalize the first letter
		* `camelCase`
---
# Math operations
* `+, -, *, /`
	* `myVar++` is the same as `myVar = myVar + 1`
	* `myVar--` is the same as `myVar = myVar - 1`
* decimal numbers / floating point numbers / floats
	* `0.009`
	* meaning any number that has a decimal point
* finding __Remainder__
	* use the `%` operator
* Argumented math operations
	* `+=, -=, *=, /=`
	* `a += 12` is the same as `a = a + 12`. same for others.
---
