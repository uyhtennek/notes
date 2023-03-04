# Introduction

A *regular expression (or RE)* allows us to check if a particular string matches or contains a given regular expression, or pattern of string
* works on Unicode strings (str) or 8-bit strings (bytes)
	* but cannot be mixed
	* cannot match a Unicode string with a byte pattern, or vice-versa
	* also apply to when asking for a substitution
* most regular expression operations are available as module-level functions and methods
	* `re` module it a standard library
	* `regex` module is a third-party library that offers additional functionality
	* also Regex can actually be used independently of any programming language

```python
import re
```
___

# Raw string
it's a type of string in Python, prefixed with an letter `r`
it tells Python not to handle back slashes `\` in any special way

```python
1. print('\tTab')
2. print(r'\tTab')

# result
1.     Tab
2. \tTab
```

* we need this because the back slash `\` matters for regular expression, but Python would do its thing first before passing the string to our regular expression operators if we don't do so
___

# `re.compile()`

* allow us to store the patterns into a variable
	* so we can reuse it to perform multiple searches

```python
# define a pattern
pattern = re.compile(r'abc')  # it's case sensitive

# search for the pattern in a string
matches = pattern.finditer(text_to_search)

# print results
for match in matches:
	print(match)

# result
<_sre.SRE_Match object; span=(1, 4), match='abc'>
```
* `span` is the beginning and the end index of the match
	* useful for getting the exact match
	* `text_to_search[1:4]` would give `'abc'`
___

# MetaCharacters
`. ^ $ * + ? { } [ ] \ | ( )`

There are *MetaCharacters*, that are need to be escaped
* `re.compile(r'.').finditer()` would match almost everything
* it's because `'.'` is a special character in regular expression
* if we want to search for `'.'`, we need to type `re.compile(r'\.')`
	* eg. `re.compile(r'coreyms\.com')`
___

# Snippet

```
--- Characters ---
.     - Any Character Except New Line '\n'
\d    - Digit (0-9)
\D    - Not a Digit
\w    - Word Character (a-z, A-Z, 0-9, _)
\W    - Not a Word Character
\s    - Whitespace (space ' ', tab '\t', newline '\n')
\S    - Not Whitespace

---- Anchors ----
\b    - Word Boundary          // eg. Ha HaHa - re.compile('\bHa') gives the first 2 Ha
\B    - Not a Word Boundary    // eg. Ha HaHa - re.compile('\BHa') gives the last Ha
^     - Beginning of a String  // re.compile('^Start')
$     - End of a String        // re.compile('end$')
```
* basically any capital letter means *not*
* *Anchors* don't match any character
	* but match positions before or after characters
___

# eg. Phone number

let say we want to match phone numbers in a String
where phone numbers are like this:
* `321-555-4321`
* `123.555.1234`

how to match?
`re.compile(r'\d\d\d.\d\d\d.\d\d\d\d')`
___

# Character set

using the same phone number example
the `'.'` would match any character, but what if we only want to match `'-'` or `'.'`?

we use
`re.compile(r'\d\d\d[-.]\d\d\d[-.]\d\d\d\d')`

* notice that we don't need to escape the characters inside the square brackets `[]` this time
	* although we can still excape these characters if we like
* what ever inside the square brackets `[]` still make it match one character only
	* phone number like `321--555-4321` won't match
	* even a rather long character set like `[A-Za-z0-9-.]` still match one character only
___

# eg. 800 or 900 numbers

if we want to match only phone numbers that start with `800` or `900`
`re.compile(r'[89]00[-.]\d\d\d\[-.]\d\d\d\d')`
___

# Character range

dash `-` is a special character as well
* when it's put at the beginning or end (in a character set), it'll just match the literal `'-'` character
* when placed between values, it can specify a range of values
	* `[1-5]` matches digits between `'1'` and `'5'`
	* `[a-z]` matches characters between letters `'a'` and `'z'`
	* `[a-zA-Z]` matches characters between letters `'a'` and `'z'` and `'A'` and `'Z'`
___

# Caret in character set

Caret `'^'` means the beginning of a String when placed in a raw string
but when placed in a character set, it means not
* `[^a-zA-Z]` matches any character that is not in the ranges `a-z` and `A-Z`
* eg. `re.compile(r'[^b]at')`
___

# Quantifier
so far we introduced how to match a single character using special characters or character sets, but how do we match multiple characters at a time?

```
*      - 0 or More
+      - 1 or More
?      - 0 or 1
{3}    - Exact Number
{3,4}  - Range of Numbers (Minimum, Maximum)
```

using the phone number example again, it'd be a good place to use the curly braces `{}` quantifier
`re.compile(r'\d{3}.\d{3}.\d{4}')`

but sometimes we don't know how many characters with the same type in a row, so we'll need the others quantifiers

eg.
```
1. Mr. Schafer
2. Mr Smith
3. Ms Davis
4. Mrs. Robinson
5. Mr. T
```

* `re.compile(r'Mr\.')` gives only 1 and 5, not 2
	* to also get the 2, we'll need `re.compile(r'Mr\.?')`
	* either that position have `'.'` or not would match
* `re.compile(r'Mr\.?\s[A-Z]\w+')` matches 1 and 2, but not 5
	* we should use the asterisk `*` quantifier instead to match 1, 2, and 5
	* `re.compile(r'Mr\.?\s[A-Z]\w*')`
___

# Group
groups allow us to match several different patterns

* to create a group, we use parentheses `()`
* using the above example, we don't really have a neat way to match `'Mr', 'Ms', 'Mrs'`
	* the solution would be using a group
	* `re.compile(r'M(r|s|rs)\.?\s[A-Z]\w*')`
		* the vertical bar `|` means *or*
		* or maybe `re.compile(r'(Mr|Ms|Mrs)\.?\s[A-Z]\w*')` was better
___

# eg. email

```
1. CoreyMSchafer@gmail.com
2. corey.schafer@university.edu
3. corey-321-schafer@my-work.net
```

1. let match the 1st email first
    `re.compile(r'[a-zA-Z]+@[a-zA-Z]+\.com')`
2. now build it up to match the 2nd email as well
    `re.compile(r'[a-zA-Z.]+@[a-zA-Z]+\.(com|edu)')`
3. finally the 3rd email
    `re.compile(r'[a-zA-Z0-9.-]+@[a-zA-Z-]+\.(com|edu|net)')`

and surely we can find some already written regular expression online for us to use
let's try reading one of them
* `[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+`
___

# Capture information from groups

eg. URLs
```
1. https://www.google.com
2. http://coreyms.com
3. https://youtube.com
4. https://www.nasa.gov
```

To match the above URLs, we can use
`re.compile(r'https?://(www\.)?\w+\.\w+')`

Now say we want to capture the *domain name* and the *top-level domain*
* *top-level domain* means the `.com`, `.gov`, etc.
* the first thing to do is we need to surround the part that we want to capture with a group
	* `re.compile(r'https?://(www\.)?(\w+)(\.\w+)')`
* now how many groups are there?
	* actually there are 4 groups
	* the entire match is captured by a group 0 
* we can access each group through a `group()` method

```python
pattern = re.compile(r'https?://(www\.)?(\w+)(\.\w+)')
matches = pattern.finditer(urls)

for match in matches:
	print(match.group(0))

# result
https://www.google.com
http://coreyms.com
https://youtube.com
https://www.nasa.gov
```

how about printing `match.group(1)`?
```
// result
www.
None
None
www.
```

and you get what happen when printing `match.group(2)` and `match.group(3)`
```
// match.group(2)
google
coreyms
youtube
nasa

// match.group(3)
.com
.com
.com
.gov
```
___

# back reference
* it just means we can substitutes groups in a match with other text
* useful if we want to reformat some text files

```python
urls = '''
https://www.google.com
http://coreyms.com
https://youtube.com
https://www.nasa.gov
'''

pattern = re.compile(r'https?://(www\.)?(\w+)(\.\w+)')
subbed_urls = pattern.sub(r'\2\3', urls)

print(subbed_urls)

#result
google.com
coreyms.com
yotube.com
nasa.gov
```
___

# find methods
besides the `finditer()` method that the above examples use
`re` module also provides some more find methods

* `finditer()`
	* returns a [[Python Syntax#~~Arrays~~ Lists|list]] of match [[Python Syntax#Objects|objects]] with extra information and functionality
	* the common one
* `findall()`
	* just returns the matches as a list of Strings
	* if it was matching groups, it'd only return the groups only, but not any other character
		* if it was matching multiple groups, it'd return a list of [[Python Syntax#Tuples|tuples]] instead
		* where the tuples contain the groups
* `match()`
	* check if the regular expression matches at the **beginning** of a String
		* not gonna work if we are matching some text at the middle or the end of a String
	* doesn't return a list like `finditer()` or `findall()`
	* just return the first match object
		* or `None` if there isn't a match
* `search()`
	* much like `match()`, except it can find a match at the beginning / middle / end of a String
	* also just return the first match object
		* or `None` if there isn't a match
___

# flags

if we want to find a word `Start`, but any letter in `Start` can be either upper case or lower case
normally we would do something like this `re.compile(r'[Ss][Tt][Aa][Rr][Tt]')`

Or, we can make use of a *ignore case flag*
`re.compile(r'start', re.IGNORECASE)`

and there is shorthands for these flags
`re.compile(r'start', re.I)`

here are another 2 commonly used flags
* multi-line flag
	* allows us to use the caret sign `^` and the dollar sign `$` to match the beginning and the end of each line in a multi-line String
	* rather than the beginning or the end of the String
* verbose flag
	* allows us to add whitespace and add comments directly within our pattern
	* so complex patterns can be easier to understand
___
