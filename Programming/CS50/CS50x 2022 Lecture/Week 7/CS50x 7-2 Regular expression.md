# Google Form - Response validation

In Google Form, if the question is a *Short answer* type of question,
we actually can validate the user's answer
* to enable it, click the 3 dots at the bottom right corner of the question panel and click *Response validation*
* it'd open some drop down options
	* in this example that validate the response must be text and in Email format
![[Google-Form-response-validation.png]]

there is another option called *Regular expression*
* to check if the user input an email, using this option
* we would type `.+@.+\..+` as Pattern
	* and if we want the email domain have to be .edu
	* we would type `.+@.+\.edu`
![[Google-Form-response-validation-re.png]]

these very cryptic part is what's called *regular expression*
___

# Regular expression
see more in [[re - Regular expression operations]]

it's sort of like a mini-language built into Python, JavaScript, Java, etc., that allow us to express patterns in a standardized way

continuing tackle the [[CS50x 7-1 Playing with Data#Futher Cleaning Data|The Office problem]]
how do we make use of regular expression to solve the problem?
```python
import csv
import re

counter = 0

with open("favorites.csv", "r") as file:
	reader = csv.DictReader(file)
	for row in reader:
		title = row["title"].strip().upper()
		if re.search("OFFICE", title):
			counter += 1
```

* `search()` is a function from the regular expression `re` library
	* first parameter is a *pattern* to serach for
	* second parameter is the String to search in

but the above logic bascially is just the same as using `if "OFFICE" in title:` as before
to improve it, we would type
```python
...
		if re.search("^(OFFICE|THE OFFICE)$", title):
			counter += 1
```
* it means the word `OFFICE` or `THE OFFICE` needs to be at the beginning or the end of `title`
* to allow a little bit of typo, say if the user typed `Thevoffice`
	* we can use `THE.OFFICE` to replace `THE OFFICE`
___
