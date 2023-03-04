[Columns · Bootstrap v5.2 (getbootstrap.com)](https://getbootstrap.com/docs/5.2/layout/columns/)
___

# Alignment
Use [[CSS Flexbox#Alignment and justification between items|flexbox alignment]] utilities to align columns

**Vertical alignment**
* can set `align-items-` classes for the `row` element
	* `align-items-start` / `align-items-center` / `align-items-end`
* or set `align-self-` classes for the `col` elements
	* `align-self-start` / `align-self-center` / `align-self-end`
	* if we want the columns to be aligned differently

```html
<div class="container text-center">
	<div class="row align-items-start">
		<div class="col">1 of 3</div>
		<div class="col">2 of 3</div>
		<div class="col">3 of 3</div>
	</div>
</div>
```

```html
<div class="container text-center">
	<div class="row">
		<div class="col align-self-start">1 of 3</div>
		<div class="col align-self-center">2 of 3</div>
		<div class="col align-self-end">3 of 3</div>
	</div>
</div>
```

**Horizontal alignment**
* we also have the `justify-content-` classes for the `row` element
	* `justify-content-start` / `justify-content-center` / `justify-content-end` /
	* `justify-content-between` / `justify-content-around` / `justify-content-evenly`

```html
<div class="container text-center">
  <div class="row justify-content-start">
    <div class="col-4">One of two columns</div>
    <div class="col-4">One of two columns</div>
  </div>
  <div class="row justify-content-center">
    <div class="col-4">One of two columns</div>
    <div class="col-4">One of two columns</div>
  </div>
  <div class="row justify-content-end">
    <div class="col-4">One of two columns</div>
    <div class="col-4">One of two columns</div>
  </div>
  <div class="row justify-content-between">
    <div class="col-4">One of two columns</div>
    <div class="col-4">One of two columns</div>
  </div>
  <div class="row justify-content-around">
    <div class="col-4">One of two columns</div>
    <div class="col-4">One of two columns</div>
  </div>
  <div class="row justify-content-evenly">
    <div class="col-4">One of two columns</div>
    <div class="col-4">One of two columns</div>
  </div>
</div>
```
___

# Column wrapping
If more than 12 columns are placed within a single row, each group of extra columns will, as one unit, wrap onto a new line.

```html
<div class="container">
  <div class="row">
    <div class="col-9">.col-9</div>
    <div class="col-4">.col-4<br>
    Since 9 + 4 = 13 &gt; 12,
    this 4-column-wide div gets wrapped onto a new line as one contiguous unit.</div>
    <div class="col-6">.col-6<br>
    Subsequent columns continue along the new line.</div>
  </div>
</div>
```
___

# Column breaks
Breaking columns to a new line in flexbox requires a small hack:
add an element with `width: 100%` wherever we want to apply the wrapping
* normally we would use multiple `row` elements
	* but it's not as convenient as using column breaks

```html
<div class="container text-center">
	<div class="row">
		<div class="col">.col</div>
		<div class="col">.col</div>
		
		<!-- Force next columns to break to new line -->
		<div class="w-100"></div>
		
		<div class="col">.col</div>
		<div class="col">.col</div>
	</div>
</div>
```

* Also we can apply this break only at specific breakpoints with [responsive display utilities](https://getbootstrap.com/docs/5.2/utilities/display/)
	* if we want to hide the column break except for `md` breakpoint and up, we need to write the column break as follow `<div class="w-100 d-none d-md-block">`
___

# Reordering
see [[Bootstrap Utilities - Flex#Order]]

Use `order`- classes to control the visual order of the columns
* it's responsive so we can set the `order` by breakpoint
* support for `1` through `5`
	* columns with no `order` are the first, followed by columns with `order-1`
	* so `5` is the last columns in visual order

```html
<div class="container text-center">
    <div class="row">
      <div class="col">
        First in DOM, no order applied
      </div>
      <div class="col order-5">
        Second in DOM, with a larger order
      </div>
      <div class="col order-1">
        Third in DOM, with an order of 1
      </div>
    </div>
</div>
```

* we also have responsive `order-first` and `order-last` classes
	* they are equivalent to `order: -1` and `order: 6`, respectively
	* `order-first` comes before the unordered columns
* can also be intermixed with the numbered `order-*` classes as needed
___

# Offsetting columns
we can offset grid columns in two ways
* using responsive `offset-` grid classes
	* predefined in size to match columns
* using [margin utilities](https://getbootstrap.com/docs/5.2/utilities/spacing/)
	* more useful for quick layouts where the width of the offset is variable


**Offset classes**
Move columns to the right using `offset-` classes
* these classes increase the left margin of a column by `*` columns
* it also can have breakpoints, `offset-{breakpoint}-`

```html
<div class="container text-center">
	<div class="row">
		<div class="col-3 offset-3">.col-3 .offset-3</div>
		<div class="col-3 offset-3">.col-3 .offset-3</div>
	</div>
</div>
```

We may need to reset offsets
* if we have offsets in a smaller breakpoint but want to remove it in a larger breakpoint

```html
<div class="container text-center">
	<div class="row">
		<div class="col-sm-5 col-md-6">.col-sm-5 .col-md-6</div>
		<div class="col-sm-5 offset-sm-2 col-md-6 offset-md-0">
			.col-sm-5 .offset-sm-2 .col-md-6 .offset-md-0
		</div>
	</div>
</div>
```


**Margin utilities**
We can use margin utilities like `.me-auto` to force sibling columns away from one another
* require Bootstrap v4, where Bootstrap applied flexbox

```html
<div class="container text-center">
	<div class="row">
		<div class="col-4">.col-4</div>
		<div class="col-4 ms-auto">
			.col-4 .ms-auto<br>
			offset to the end of the row
		</div>
	</div>
	<div class="row">
		<div class="col-4 me-auto">
			.col-4 .me-auto<br>
			offset the next column to the end of the row
		</div>
		<div class="col-4">.col-4</div>
	</div>
</div>
```
___

# Standalone column classes
The `col-*` classes can also be used outside of a `row`, to give an element a specific width.
Whenever a column is not a direction children of a row, the paddings are omitted.

```html
<div class="col-3">
	.col-3: width of 25%
</div>
<div class="col-sm-9">
	.col-sm-9: width of 75% above sm breakpoint
</div>
```

* the classes can be used together with utilities
___
