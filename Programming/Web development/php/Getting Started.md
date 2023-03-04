[PHP: Getting Started](https://www.php.net/manual/en/getting-started.php)
___

# What Is PHP?
*Hypertext Preprocessor*

eg.
```html
<!DOCTYPE html>
<html>
	<head>
		<title>Example</title>
	</head>
	<body>
		<?php
			echo "Hi, I'm a PHP script!";
		?>
	</body>
</html>
```

* PHP code is enclosed in special start and end processing instructions `<?php ?>`
* this code is actually executed on the server
	* it then generates HTML which is then sent to the client
___

# What Can PHP Do?
Anything

it mainly focuses on server-side scripting
* eg. collect from data, generate dynamic page content, send and receive cookies, ....

PHP scripts mostly do three things
* Server-side scripting
	* PHP's most traditional and main target
	* the server needs to install PHP
* Command line scripting
	* we can run PHP script without any server or browser, but the PHP parser
* Writing desktop applications
	* PHP probably is not the very best language for this
	* there is a PHP extension, PHP-GTK for doing this

PHP is not limited to output HTML
* but also images, PDF files, Flash movies, etc.
* it can also output text
	* eg. XTML, XML file, etc.
* PHP can autogenerate these files, then save them in the file system

PHP supports a wide range of databases
* eg. simply using MySQL
* eg. using an abstraction layer like PDO
* eg. connecting to any database supporting ODBC

PHP also supports talking to other services using protocols such as LDAP, IMAP, SNMP, NNTP, POP3, HTTP, COM, .....

PHP can do text processing
* eg. via Perl compatible regular expressions, PCRE
* eg. parse and access XML documents
	* it uses libxm2, and XML extensions SimpleXML, XMLReader, XMLWriter

and more features....
___

# Installation

**Unix systems**
```bash
sudo apt install php    # install php
php -v                  # check php version
```

**Mac**
```bash
brew install php@8.2
```

**Windows**
1. download the binaries
2. set the PATH
* see [Developing on Windows, 2016 Edition — Chris Tankersley](https://ctankersley.com/2016/11/13/developing-on-windows-2016/)
___
