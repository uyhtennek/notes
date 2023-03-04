# Arrays
> Arrays allow you to store several pieces of data in one place.

```javascript
var myArray = ["John", 23];
var nestedArray = [["the universe", 42], ["everything", 101010]];

// accessing a data in an array
var myData = myArray[0];  // result is "John"
var myData = myArray[1];  // result is 23

// same thing for modifying an array data
myArray[1] = 45;
```
---
# Multi-Dimensional Arrays
```javascript
var myArray = [[1,2,3], [4,5,6], [7,8,9], [[10,11,12], 13, 14]]; // 3 layer
var myData = myArray[0][0];

// myArray[0] = [1,2,3]
// myArray[0][0] = 1
```
---
# push()
add element(s) to the end of an array
```javascript
var myArray = ["Stimpson", "J", "cat"];
myArray.push(["happy", "joy"]);

// myArray = ["Stimpson", "J", "cat", ["happy", "joy"]]
```
---
# pop()
remove the final element of an array
```javascript
var myArray = [1, 2, 3];
var remove = myArray.pop();

// remove = 3
// myArray = [1, 2]
```
---
# shift()
remove the first element of an array
```javascript
var myArray = ["Stimpson", "J", ["cat"]];
var remove = myArray.shift();

// remove = "Stimpson"
// myArray = ["J", ["cat"]]
```
---
# unshift()
add element(s) to the beginning of an array
```javascript
var myArray = ["Stimpson", "J", "cat"];
myArray.shift(); // now myArray equals ["J", "cat"]
myArray.unshift("Happy");

// myArray = ["Happy", "J", "cat"]
```
---
# eg. Shopping List
```javascript
var myList = [["cereal", 3], ["milk", 2], ["bananas", 3], ["juice", 2]];
```
---
