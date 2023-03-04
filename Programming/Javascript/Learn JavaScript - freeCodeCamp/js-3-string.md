# String
* enclosed by `""` or `''` or `backticks`
* use `\` to escape literal quote
	* `"I am a \"double quoted\" string."`
	* if used `''`, it is no need to use the escape character `\`
* using `backticks` allow us to use both `''` and `""` in a string
```javascript
var myStr = `'this is a "doublie quoted" string.'`;
```
---
# Escape Sequences
* `\'` - single quote
* `\"` - double quote
* `\\` - backslash
* `\n` - new line
* `\r` - carriage return
* `\t` - tab
* `\b` - backspace
* `\f` - form feed
---
# Concatenating strings
* using `+` operator
```javascript
var myStr = "I come first. " + "I come second.";

// myStr becomes
"I come first. I come second."
```
* using `+=` operator
```javascript
var myStr = "I come first. ";
myStr += "I come second.";

// myStr becomes
"I come first. I come second."
```
* concatenate string with __variables__
```javascript
var myName = "Kenneth";
var myStr = "Hello. I am " + myName + ".";

// myStr becomes
"Hello. I am Kenneth."
```
---
# Find the length of string
use the `.length` property
```javascript
var myStr = "Ada";
var myStrLength = myStr.length;
// myStrLength is 3
```
---
# Bracket Notation for string
```javascript
var lastName = "Lovelace";
console.log("first letter is: " + lastName[0]);
console.log("second letter is: " + lastName[1]);
console.log("last letter is: " + lastName[lastName.length - 1]);
console.log("second last letter is: " + lastName[lastName.length - 2]);

// result
// first letter is: L
// second letter is: o
// last letter is: e
// second last letter is: c
```
---
# String immutability
> individual characters of a string literal cannot be changed
```javascript
var myStr = "Jello World";
myStr[0] = "H";        // this give error
myStr = "Hello World"; // this is fine
```
---
