# Simplest tags

`<b>`, `</b>`
<b>bold text</b>

`<i>`, `</i>`
<i>italic text</i>

`<u>`, `</u>`
<u>underlined text</u>

`<p>`, `</p>`
<p>paragraph</p>
* leave space above and below
* hitting Enter doesn't work because HTML doesn't care about whitespace
	* the two text would just slam together and become one

`<hX>`, `</hX>`
headers
* X = 1, 2, 3, 4, 5, or 6
* which I don't quite like demonstrating here though....

`<ul>`, `</ul>`
un-ordered list
<ul><li>bulleted list</li></ul>

`<ol>`, `</ol>`
ordered list
* change the `start` attribute to set the starting number
	* by default it's `1`
<ol start=6><li>list with numbers</li><li>next list item</li></ol>

`<li>`, `</li>`
list item
* placed inside `<ul>` or `<ol>` element

`<hr>`
* thematic break
<hr style="margin-top: 0.5em;">

# Table

`<table>`, `</table>`
beginning and end of a table
`<tr>`, `</tr>`
beginning and end of a *table row*
`<td>`, `</td>`
beginning and end of a column, *table data*

<table>
	<tr>
		<td>1</td>
		<td>2</td>
		<td>3</td>
		<td>4</td>
	</tr>
	<tr>
		<td>2</td>
		<td>4</td>
		<td>6</td>
		<td>8</td>
	</tr>
	<tr>
		<td>3</td>
		<td>6</td>
		<td>9</td>
		<td>12</td>
	</tr>
	<tr>
		<td>4</td>
		<td>8</td>
		<td>12</td>
		<td>16</td>
	</tr>
</table>

* it's ugly but the style would eventually be taken care of by CSS
___

# Form

one typical use case is logging in a web page
form is what's getting our inputted username and password

`<form>`, `</form>`
beginning and end of an HTML form

`<input name=X type=Y />`
* placed inside of `<form>`
* the elements that we're actually typing our usernames and passwords into
	* or the radio buttons we're ticking
	* or the check boxes
* comprise bascially each row of the form
* also notice that this is a *self-closing tag*
	* it doesn't have a partner closing tag
		* because all the information is contatined inside the tag itself and its attributes
	* so it has this weird syntax `<... />`

Here is a form
<div>
	<form>
		Text: <input name="a" type="text" /><br/>
		Password: <input name="b" type="password" /><br/>
		Radio: <input name="c" type="radio" /><br/>
		Checkbox: <input name="d" type="checkbox" /><br/>
		Submit: <input name="e" type="submit" value="Send" /><br/>
		Dropdown:
		<select>
			<option disable selected>Sport</option>
			<option>Basketball</option>
			<option>Soccer</option>
			<option>Ultimate Frisbee</option>
		</select>
	</form>
</div>

* difference between Text and Password is that text in Password won't get shown
* Although the Submit button doesn't do anything, by default the page would just refresh
	* we can config the Submit button to do something else
	* using JavaScript or PHP
___

# Division

`<div>`, `</div>`
beginning and end of an arbitrary HTML page division
* there is no visual break, no line. it doesn't separate chunk automatically
* it just gives a piece of space on the web page
___

# Hyperlink

`<a href=X>`, `</a>`
* `a` stands for *anchor*, that how we call hyperlink in HTML
* `href=X` means go to web page `X`
	* it can be an internal link or an external link
	* surely external links would need `http://` or `https://`

<a href="https://cs50.harvard.edu">CS50's website</a>

there is another HTML tag called `<link>`
* which is not a hyperlink

`<img src=X ... />`
* a self-closing tag
* we might be able to change the image's size by specifying `width` and `height` and other attributes in that `...`
___

# `<!DOCTYPE html>`

since HTML has been around since the early 1990s,
it has been some several revisions since then

Most recently is the one in 2014, called HTML5
it's the current sort of HTML standard

To indicate our web page is written using HTML5
we start off the HTML page with `<!DOCTYPE html>`
* we can choose not to add that
* but then we can't use any HTML5 tag
	* also we didn't touch on any HTML5 tag
___

# Comment

`<!-- comment -->`
* no closing bracket

<!-- This is a comment -->
It's invisible in Obsidian's read mode, as it's a comment
___
