# Common properties


**Color**

`border: width style color;`
* `style` could be, `dotted`, `dashed`, `solid`, `ridge`, etc.
<p style="border: 2px solid red;">border example</p>

`background-color: [keyword | #<6-digit hex>]`
* some colors are pre-defined in CSS
<p style="background-color: green">background-color example</p>

`color: [keyword | #<6-digit hex>]`
* sets the foreground color
	* usually it's text
<p style="color: green;">color example</p>

**Text**

`font-size: [absolute size | relative size]`
* can use keywords, `xx-smal`, `medium`, ...
* can use fixed points, `10pt`, `12pt`, ...
* can use percentage, `80%`, `120%`, ...
* even can base off the most recent font size, `smaller` / `larger`
<p style="font-size: 12;">font-size example</p>

`font-family: [font name | generic name]`
* There are some "web safe" fonts
* Times New Roman, Arial, Courier New, Georgia, Tahoma, Verdana, etc.
	* if you use Microsoft Office in the last 20 years, perhaps you know some already
* every web page had access to this default set of fonts even we didn't define them
<p style="font-family: Times New Roman;">font-family example</p>

`text-align: [left | right | center | justify]`
<p style="text-align: center;">text-align example</p>
___

# Table example

```css
table
{
	border: 5px solid red;
}

tr
{
	height: 50px;
}

td
{
	/* Lots being applied here! */
	
	background-color: black;
	color: white;
	font-size: 22pt;
	font-family: georgia;
	text-align: center;
	width: 50px;
}
```

```html
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
```
___
