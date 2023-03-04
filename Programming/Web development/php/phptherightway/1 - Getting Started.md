[PHP: The Right Way (phptherightway.com)](https://phptherightway.com/)
___

# php Built-in server

run a command to boot up the built-in web server
```bash
php -S localhost:8000
```
___

# Install php
on Linux

```bash
sudo apt install php
```

see [[Getting Started#Installation]]
___

# Common Directory structure
where do I put my stuff?

configuration files should not be accessible by a site's visitors, so
* put public scripts in a public directory
* put private configurations and data outside of that directory

[**Standard PHP package skeleton**](https://github.com/php-pds/skeleton)
* `DocumentRoot` should point to `public/`
* unit tests should be in `tests/`
* third party libraries should belong in `vendor/`
___

# Code Style Guide
https://phptherightway.com/#code_style_guide
read [Clean Code concepts adapted for PHP](https://github.com/piotrplenik/clean-code-php) as well

we can use [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) to detects violations of a defined set of coding standards
* [PHP Code Beautifier and Fixer](https://github.com/squizlabs/PHP_CodeSniffer/wiki/Fixing-Errors-Automatically) is included in PHP CodeSniffer to fix the code layout automatically
* [PHP Coding Standards Fixer](https://cs.symfony.com/) is another option for fixing code layout

to use PHP CodeSniffer, run a command
```bash
phpcs -sw --standard=PSR1 file.php
```
* it will show errors and describe how to fix them
* it can be helpful to include this command in a git hook

we can fix the code layout problems automatically with PHP Code Beautifier and Fixer
```bash
phpcbf -w --standard=PSR1 file.php
```
* if you are using PHP Coding Standards Fixer instead
```bash
php-cs-fixer fix -v --rules=@PSR1 file.php
```
___

# Language Highlights
https://phptherightway.com/#language_highlights

**Object-oriented Programming**
* added in PHP 5.0 (2004)
* include support for classes, abstract classes, interfaces, inheritance, constructors, cloning, exceptions, .....

**Functional Programming**
* PHP supports first-class functions, meaning that
	* a function can be assigned to a variable and invoked dynamically
	* a function can be passed as arguments to other functions
		* it's a feature called *Higher-order Functions*
	* a function can return other functions
* anonymous functions are present since PHP 5.3 (2009)

**Meta Programming**
* it's supported through mechanisms like the Reflection API and Magic Methods
* eg. `__get(), __set(), __clone(), __tostring(), __invoke()`, etc.

**Namespaces**
* it's important to namespace your code so that it doesn't collide with other libraries in any case

**Standard PHP Library**
* or *SPL* for short
* it has commonly needed datastructure classes
	* eg. stack, queue, heap, etc.
* along with their iterators
___

## Command Line Interface
useful for automating common tasks like testing, deployment, application administration, etc.

> [!warning] Don't put your CLI PHP scripts in the public web root.

eg. print PHP configuration
* it's the same as the function `phpinfo()`
```bash
php -i
```

eg. enter an interactive shell
* similar to ruby's IRB or python's interactive shell
```bash
php -a
```

eg. `hello.php`
```php
<?php
if ($argc !== 2) {
	echo "Usage: php hello.php <name>" . PHP_EOL;
	exit(1);
}
$name = $argv[1];
echo "Hello, $name" . PHP_EOL;
```

`$argc, $argv`
* argument count, argument value
* `$argv[0]` is always the name of the PHP script file, `hello.php` in this case

`exit()`
* non-zero number means the command failed
* also see [commonly used exit codes](https://www.gsp.com/cgi-bin/man.cgi?section=3&topic=sysexits)

to run the script from the command line
```bash
php hello.php
# output: Usage: php hello.php <name>
php hello.php world
# output: Hello, world
```
___

## Xdebug
the PHP's debugger

[install Xdebug](https://xdebug.org/docs/install)
* one of its most important features is [Remote Debugging](https://phptherightway.com/#xdebug)
	* it enables us to test code on a remote machine
___
