[PHP Basics](https://www.youtube.com/playlist?list=PLfdtiltiRHWHjTPiFDRdTOPtSyYfz3iLW)
* use `phpinfo` to check the PHP version and configurations
___

# `echo`
[PHP echo and print - GeeksforGeeks](https://www.geeksforgeeks.org/php-echo-print/)

```php
<?php
	echo "Hello World";            // output: Hello World
	echo "Hello", "World";         // output: Hello World
	echo "Hello" . " " . "World"   // output: Hello World
	// output: Hello WorldHello WorldHello World
?>
```

`echo "Hello World\n"` would work in command line but not in HTML page
* one solution would be using `var_dump`
* another solution would be doing `echo "Hello World" . <br>;` or `echo "<pre>Hello World\n</pre>";` or the likes

note that only `echo` can do `"Hello", "World"`
* [[#Concatination|string concatenating in PHP]] doesn't work like this
___

# `var_dump`
[PHP: var_dump](https://www.php.net/manual/en/function.var-dump.php#:~:text=var_dump%20%28PHP%204%2C%20PHP%205%2C%20PHP%207%2C%20PHP,explored%20recursively%20with%20values%20indented%20to%20show%20structure.)

eg 1
```php
<?php
$a = array(1, 2, array("a", "b", "c"));
var_dump($a);
?>
```
output
```
array(3) {
	[0]=>
	int(1)
	[1]=>
	int(2)
	[2]=>
	array(3) {
		[0]=>
		string(1) "a"
		[1]=>
		string(1) "b"
		[2]=>
		string(1) "c"
	}
}
```

eg 2
```php
<?php
$b = 3.1;
$c = true;
var_dump($b, $c);
?>
```
output
```
float(3.1)
bool(true)
```
___

## Preserve Formatting

actually the outputs of `var_dump` would not be this clean
we can use some PHP extension to format nicely
* Xdebug does this

alternatively, if we don't have that, we can use a little trick to format it nicely
```php
echo '<pre>', var_dump($a), '</pre>';
echo '<pre>' . var_dump($a) . '</pre>';
```
* here we warp the output of `var_dump` in a HTML `<pre>` tag
	* to *preserve* the formatting
* then we can get the output exactly as the above
___

# Arrays

declaring an array
```php
<?php
$names = [];       // create an empty array
var_dump($names);  // output: array(0) { } in the page

$names = ['alex', 'billy'];  // or
$names = array('alex', 'billy');
var_dump($names);  // output: array(2) {[0]=>string(4) "alex" [1]=>string(5) "billy"}
?>
```

adding an element to an array
```php
<?php
$names = ['alex', 'billy'];  // create the array
$names[] = 'dale';           // add 'dale' to the array
var_dump($names);
?>
```
___

## Associative Array
much like a dictionary

```php
<?php
$people = [
	'alex' => 26,
	'billy' => 21
];
var_dump($people);
?>
```

output
```
array(2) {
	["alex"]=>int(26)
	["billy"]=>int(21)
}
```

now if we do `echo $people[0];`
it outputs `Undefined offset: 0 in .../index.php on line 8`

what about `echo $people['dale'];`?
we get `Undefined index: dale in .../index.php on line 8`


a little bit more complicated example
```php
<?php
$users = [
	['username' => 'alex', 'email' => 'alex@codecourse.com',
	 'likes' => ['cats', 'food']],
	['username' => 'billy', 'email' => 'billy@codecourse.com',
	 'likes' => ['books', 'cats']]
];
echo $users[0]['username'];  // output: alex
?>
```

what if we do `echo $users[0]`?
we get a message, `Array to string conversion in .../index.php on line 8`
follow up by a string `Array` in the next line

adding a new user
```php
$users[] = [
	'username' => 'josh',
	'email' => 'josh@codecourse.com',
	'likes' => ['reading', 'cooking']
]
```


## Tips

we can do `$users[0] = 'alex';`
* even the data doesn't match, things will not break

we can add key-value pairs to a existing element
* `$users[1]['about'] = 'I am learning to code.';`
___

# `NULL`
In PHP, it can be either `NULL` or `null`

> [!info] Be consistent.
> Either all `NULL` or all `null`.

`NULL` happens either when
* a variable is set to `NULL`
* or a variable simply hasn't been set a value

```php
<?php
	$name = null;
	var_dump($name);  // output: NULL
?>
```

how about if we just declare a variable without setting it anything?
```php
<?php
	$name;
	var_dump($name);
?>
```
output
```
Undefined variable: name in .../index.php on line 5
NULL
```
* the same go for if we `var_dump` a variable that simply does not exist


## `unset`
it unsets a variable

```php
<?php
	$name = 'alex';
	unset($name);
	echo $name;
?>
```
* output `Undefined variable: name in .../index.php on line 7`
	* but without a `NULL` on the next line
		* it only happens for `var_dump`

note that `unset($name)` is not the same as `$name = null`
* for `$name = null;`, the `$name` variable does exist, just its value is `null`
* for `unset($name);`, the `$name` variable doesn't exist any more
* so we won't get the `Undefined variable` message if we do `$name = null;`
	* it simply outputs the value of `$name`, which is `NULL`
___

# String Concatination

```php
<?php
	$weather = 'sunny';
	
	// 1
	echo 'The current weather is ';
	echo $weather;
	
	// 2
	echo 'The current weather is ' . $weather;
	
	// 1 and 2 are the same
	// output: The current weather is sunny
?>
```

one more example
```php
<?php
	$weather = 'sunny';
	$degrees = 30.5;
	
	$status = 'The current weather is ' . $weather . ' and it\'s ' . $degrees . ' degrees.';
	echo $status;
?>
```
* note that `$degress` is implicitly converted from a float to a string by concatenating

> [!tip] `strlen`
> `strlen($status)` outputs the length of the string `$status`

___

## String Formatting

```php
<?php
	$weather = 'sunny';
	$degrees = 30.5;
	
	$status = "The current weather is $weather and it's $degrees degrees";
	echo $status;
?>
```
* note that we're using double quotes `""` but not single quotes `''` here
* then PHP will know to parse the variables as strings

now what if we want to say `it's $degrees° degrees`?
* we get a message - `Undefined variable: degrees° in .../index.php in line 6`
* and an output - `The current weather is sunny and it's degrees`
* the solution is wrapping `$degress` in curly braces `{}`
	* `it's {degress}° degrees`
___

# Type Casting

```php
<?php
	echo '1alex';       // output: 1alex
	echo (int)'1alex';  // output: 1
	var_dump((bool)'false');  // output: bool(true)
?>
```
___

# Comparison Operators

```php
<?php
	$dayOfWeek = 1;
	
	$daysOfWeek = [
		1 => 'Monday',
		2 => 'Tuesday',
		3 => 'Wednesday'
	];
	
	if (in_array($dayOfWeek, array_keys($daysOfWeek))) {
		echo $daysOfWeek[$dayOfWeek];
	} else {
		echo 'That is not a valid day.';
	}
?>
```

`array_keys($daysOfWeek)`
* return an array of keys that are in `$daysOfWeek`

`1` is `true`, `0` is `false`
* `2, -5` is also `true`
* `null` is `false`
 
`==` vs `===`
* `===` is strict comparison
	* not only the value has to be the same, the type has to be the same as well
```php
<?php
	$a = -5;
	
	$a == true;  // true
	$a === true; // false, because the type is not equal
	
	// these 2 are the same, both are false
	$a != true;
	!$a;
	
	$a !== true; // true, the value are equal, but the type are not
	$a <> true;  // true, this is actually the same as above
?>
```
___

# Arithmetic
doing math in PHP

`0 + '1'`
* PHP can automatically cast the string `'1'` as a number `1`
* as long as it's a number

`7 / 30`
* in PHP, it's not `0`, but `0.2333...`
* if we want `0`, we use `number_format(7 / 30)`
	* if we want `0.23`, so two decimal places, do `number_format(7 / 30, 2)`

`10 ** 2`
* it's the *exponential operator*
* it's the same as `10 * 10`
* eg. `10 ** 9` would be the same as `10 * 10 * ... * 10` nine times
___

# Loop

**for loop**
```php
<?php
	for (;;) { }  // this is actually a valid for loop
	
	for ($i = 1; $i <= 10; $i++) {
		echo $i;
	}
?>
```

**foreach loop**
```php
<?php
	foreach ($users as $user) {
		echo $user['username'] . '<br>';
	}
?>
```
___

# `print_r`
it basically is just an alternative to `var_dump`

```php
<?php
	$users = [
		['name' => 'alex', 'age' => 27],
		['name' => 'billy', 'age' => 23]
	];
	echo '<pre>', print_r($users), '</pre>';
?>
```
___

# Functions

eg. print a name
```php
<?php
	function fullName($firstName, $lastName = null, $seperator = ' ') {
		if (trim($firstName) === '') {
			return;   // it returns null
		}
		
		if ($lastName === null) {
			return $firstName;
		}
		
		return "{$firstName}{$seperator}{$lastName}";
	}
	
	echo fullName('Alex', 'Garrett');
?>
```

`trim($firstName)`
* strip the whitespaces from the left and right hand side of the variable

eg. adding numbers
* pass in unlimited arguments to a function
```php
<?php
	// present in PHP 7
	function add(array $numbers) {
		$total = 0;
		foreach ($numbers as $number) {
			$total += $number;
		}
		return $total;
	}
	
	echo add([5, 10, 10, 1]);
?>
```

what if we call `add(5)`?
* we get an error
```
Fatal error: Uncaught TypeError: Argument 1 passed to add() must be of the type array, integer given, called in .../index.php:5
Stack trace: #0 .../index.php(9): add(5) #1 {main} thrown in .../index.php on line 5
```
___

## `func_get_args`
actually there is an easier way to the previous example

```php
<?php
	function add() {
		var_dump(func_get_args());
	}
	echo add();
	// output: array(0) { }
?>
```

`func_get_args`
* it returns an array of the arguments passed in

now if we call `add(5, 10, 10)`, we get the output
```php
array(3) {
	[0] => int(5)
	[1] => int(10)
	[2] => int(10)
}
```

now let's recreate the `add` function
```php
function add() {
	$total = 0;
	foreach (func_get_args() as $number) {
		// protect the function
		if (!is_numeric($number)) {
			// we can use is_int if we want
			continue;
		}
		
		$total += $number;
	}
	return $total;
}
```

actually the function wouldn't break if we don't add the protection
* so calling `add(5, 10, 10, 'alex')` even without the protection, is okay
* because PHP is doing the type casting

it's for cases like `add(5, 10, 10, '1alex')`
* this will output `26`
* if we add the protection then it would be `25`

> [!info] `array_sum`
> actually PHP has a built-in function for this exact function
> ```php
> echo array_sum([5, 10, 10]);
> ```

> [!warning] note that the `func_get_args` approach is **Less Readable** than `function add(array $numbers)`

___

## Scope

eg. making use of a `config` variable
* which should be a global variable
```php
<?php
	$config = [
		'seperator' => ' '
	];
	
	function fullName($firstName, $lastName) {
		return "{$firstName}{$config['seperator']}{$lastName}";
	}
	
	echo fullName('Alex', 'Garrett');
?>
```

this would give an error
```
Notice: Undefined variable: config in .../index.php on line 8
AlexGarrett
```
* why PHP didn't just bring variables into the functions automatically?
	* because if we want another `$config` in the function, it creates conflict

solution 1 - bring it into the function
```php
function fullName($firstName, $lastName) {
	global $config;
	return "{$firstName}{$config['seperator']}{$lastName}";
}
```
* call `fullName('Alex', 'Garrett')`, outputs `Alex Garrett`

solution 2 - pass in `config` as an argument
```php
function fullName($firstName, $lastName, $config) {
	return "{$firstName}{$config['seperator']}{$lastName}";
}
```
* not a very clean solution because we would need to call the function by `fullName('Alex', 'Garrett', $config)`

solution 3 - perhaps the best solution
```php
function fullName($firstName, $lastName) use ($config) {
	return "{$firstName}{$config['seperator']}{$lastName}";
}
```
* actually this isn't actually gonna work
	* `Parse error: syntax error, unexpected 'use'(T_USE), expecting '{' in ...`
* this is the correct use of the function
```php
<?php
	$config = ['seperator' => '_'];
	
	$fullName = function ($firstName, $lastName) use ($config) {
		return "{$firstName}{$config['seperator']}{$lastName}";
	};
	echo $fullName('Alex', 'Garrett');  // output: Alex_Garrett
?>
```
___

# Breaking up files

```php
// file: hello.php
<?php
	echo "Hello";
?>
```

```php
// file: index.php
<?php
	include 'hello.php';
?>
```
* output `Hello`

`require` almost does the same thing and output `Hello` as well
```php
// file: index.php
<?php
	require 'hello.php';
?>
```
* the difference between `include` and `require` is, if there is an error in `hello.php`, the rest of the `index.php` is not going to execute
* or if the line `require 'hello.php';` had error
	* eg. maybe we asked for a file that doesn't exist, `require 'goodbye.php';`

> [!tip] `require` or `include`?
> Typically we would want to use `require`, because if there are mistakes in the first place, we rarely would want the code following to run.

now, what happen if we use `include` or `require` twice?
```php
// file: index.php
<?php
	include 'hello.php';
	include 'hello.php';
?>
```
* then we get the output in `hello.php`, twice

but we can force only include the file once
```php
<?php
	include_once 'hello.php';
	include_once 'hello.php';
?>
```

eg. breaking up files for reusing functions
```php
// file: functions/user.php
<?php
	function fullName($firstName, $lastName) {
		return "{$firstName} {$lastName}";
	}
?>
```
```php
// file: index.php
<?php
	require_once 'functions/user.php';
	echo fullName('Alex', 'Garrett');
?>
```
```php
// file: users.php
<?php
	require_once 'functions/user.php';
	echo fullName('Alex', 'Garrett');
?>
```
___

# Superglobal
basically, they are global variables built-in to PHP

let's say our `index.php` look like this
```php
<?php
	var_dump($_GET);
?>
```

`$_GET`
* the superglobal
* it gets URL parameters via GET method

now if we visit `<domain>:<port>/index.php?slug=learn-php`,
we can see `array(1){["slug"]=>string(9)"learn-php"}`
* `$_GET['slug']` would give us `"learn-php"`


## eg. Search

```php
<?php
	$page = $_GET['page'];
	$searchTerm = $_GET['search'];
	$pages = 10;
	
	echo '<h3>Searching for: ' . $searchTerm . '</h3>';
	echo '<p>You are on page ' . $page . '</p>';
	
	// <a> tag to switch to any one page
	for ($i = 0; $i <= $pages; $i++) {
		echo '<a href="?search=' . $searchTerm . '&page=' . $i . '">' . $i . '</a>';
	}
?>
```

but it assumes the URL to have the GET parameters
otherwise it would give errors

a solution to this would be using `isset` to check if the GET parameters are there
```php
<?php
	if (isset($_GET['search'])) {
		$searchTerm = $_GET['search'];
		echo '<h3>Searching for: ' . $searchTerm . '</h3>';
	}
?>
```

`isset($_GET['search'])`
* if we have more things to check we can put them in as well
	* eg. `isset($_GET['search'], $_GET['page'])`


## eg. Login

```html
<!-- file: index.php -->
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
		<form action="signin.php" method="post">
			<label for="username">Username</label>
			<input type="text" name="username" id="username">
			
			<label for="password">Password</label>
			<input type="password" name="password" id="password">
			
			<button type="submit">Sign in</button>
		</form>
	</body>
</html>
```
```php
// file: signin.php
<?php
	echo $_POST['username'], '<br>';
	echo $_POST['password'];
?>
```
___

# Ternary Operator

```php
<?php
	$page = isset($_GET['page']) ? $_GET['page'] : 1;
	echo $page;
?>
```

actually there is another way
```php
<?php
	$page = $_GET['page'] ?? 1;
	echo $page;
?>
```

`??`
* null check operator
* if `$_GET['page']` doesn't have a value, use `1`

and there is one more trick
```php
<?php
	$balance = 0;
	$availableBalance = $balance ?: 'zero';
	echo 'Your available balance is ' . $availableBalance;
?>
```

`?:`
* false check operator
* if `$balance` is false, use `'zero'`
___

# PHP Functions

`int strlen(string $string)`
* returns the length of the given string
* returns `0` if the string is empty

`string strtoupper(string $string)`
* convert `$string` to all upper case
* similarly `strtolower` convert `$string` to all lower case

`string trim(string $str [, string $character_mask = " \t\n\r\0\x0B"])`
* remove whitespaces to the left and right hand side of the `$str`
* we can specify what to strip from `$str` as well
	* `trim($str, '.');`
		* this get rid of all the `.` to the left and right, but won't remove whitespaces
* similarly we have `ltrim` and `rtrim`
	* `ltrim` removes the characters from the beginning of the string
	* `rtrim` removes the characters from the end of the string

`string substr(string $string, int $start [, int $length])`
* returns a portion of string
* if `$length` is greater than the length of `$string`, it would still work like normal
	* it gets the full `$string`
* eg. grabbing half of a string, `substr('alexander', 0, ceil(strlen($name) / 2));`

`bool empty(mixed $var)`
* check if a variable is considered to be empty
* `mixed` means any type
* eg. a more robust GET
	* consider we're requesting from the URL `<domain>:<port>/index.php?page=     `
```php
<?php
	$page = $_GET['page'] ?? 1;
	if (empty(trim($page))) {
		$page = 1;
	}
	echo $page;
?>
```

`string number_format(float $number [, int $decimals = 0])`
`string number_format(float $number, int $decimals = 0, string $dec_point = ".", string $thousands_sep = ",")`
* `number_format(5.5)` would be rounded up to `6`
	* it rounds to the nearest
* `number_format(1000000000, 2)` would output `1,000,000,000.00`
	* because it is the `$dec_point`'s and `$thousands_sep`'s defaults

`void header(string $string [, bool $replace = true [, int $http_response_code]])`
* used to redirect users
	* it sends a raw HTTP header to the client
```php
// file: index.php
<?php
	header('Location: page.php');  // redirect to page.php
?>
```
* note that we can't output anything if we want to use `header`
```html
<html>  <!-- this will cause an error -->
<?php
	header('Location: http://www.example.com/');
	exit;
?>
```
___
