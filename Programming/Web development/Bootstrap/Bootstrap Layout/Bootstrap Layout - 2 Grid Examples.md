[Grid Template · Bootstrap v5.2 (getbootstrap.com)](https://getbootstrap.com/docs/5.2/examples/grid/)
[Grid system · Bootstrap v5.2 (getbootstrap.com)](https://getbootstrap.com/docs/5.2/layout/grid/#auto-layout-columns)
___

# Grid options

There are 6 tiers to the Bootstrap grid system, one for each range of devices they support.
* Each tier starts at a minimum viewport size and automatically applies to the larger devices
* unless overridden

| | **xs**<br><576px | **sm**<br>&ge;576px | **md**<br>&ge;768px | **lg**<br>&ge;992px | **xl**<br>&ge;1200px | **xxl**<br>&ge;1400px |
| --- | --- | --- | --- | --- | --- | --- |
| Container<br>`max-width` | None<br>(`auto`) | `540px` | `720px` | `960px` | `1140px` | `1320px` |
| Class prefix | `.col-` | `.col-sm-` | `.col-md-` | `.col-lg-` | `.col-xl-` | `.col-xxl-` |

Use class `col` if we want the column just take as much space as it can
* fill up the space that is not taken by the next column in a row of a container

Use class `col-1` if we want the column takes 1 out of 12 space of the container
* maximum is `col-12`
	* think of 12 as the total number of columns available for each type of Bootstrap's containers
	* containers have space for 12 columns only, the 13th would get to a new row
* therefore if we use more than just one predefined grid columns provided by Bootstrap
	* grid columns should add up to twelve or less for a single horizontal block
	* more than that, columns start stacking no matter the viewport
___

# Equal-width
Every column in a row has the same width, no matter the viewport size.

```html
<div class="container text-center">
	<div class="row">
		<div class="col">1 of 2</div>
		<div class="col">2 of 2</div>
	</div>
</div>
```

if we want the columns to be a certain size we can set that for each column
alternatively we can set that in the parent element, the row element
```html
<div class="container text-center">
	<div class="row row-cols-3">
		<div class="col">1 of 3</div>
		<div class="col">2 of 3</div>
		<div class="col">3 of 3</div>
	</div>
</div>
```

* we specify the number of columns in the row class [`row-cols-*`](https://getbootstrap.com/docs/5.2/layout/grid/#row-columns)
	* it will control the width of its children
* similarly we have `row-cols-{breakpoint}-`
	* see what is `cols-{breakpoint}-` in a [[#Stacked to horizontal|later section]]
___

# Setting one column width
We can set the width of one column and have the sibling columns automatically resize around it.
* we may use predefined grid classes, grid mixins, or inline widths

```html
<div class="container text-center">
	<div class="row">
		<div class="col">1 of 3</div>
		<div class="col-6">2 of 3(wider)</div>
		<div class="col">3 of 3</div>
	</div>
</div>
```
___

# Variable width content
Use `col-auto` class to size columns based on the natural width of their content

```html
<div class="container text-center">
	<div class="row justify-content-center">
		<div class="col">1 of 3</div>
		<div class="col-auto">Variable width content</div>
		<div class="col">3 of 3</div>
	</div>
</div>
```

* we also have `col-{breakpoint}-auto`, which is the [[#Stacked to horizontal|responsive version]] of `col-auto`
___

# Stacked to horizontal
Use `col-{breakpoint}-` classes to make the columns in a row starts out stacked and becomes horizontal when the viewport's width meets the breakpoint
* if the viewport width is smaller than the `{breakpoint}` width, the column's `width` becomes `100%` and stacks under the last column

```html
<div class="container text-center">
	<div class="row">
		<div class="col-sm-8">col-sm-8</div>
		<div class="col-sm-4">col-sm-4</div>
	</div>
	<div class="row">
		<div class="col-sm">col-sm</div>
		<div class="col-sm">col-sm</div>
		<div class="col-sm">col-sm</div>
	</div>
</div>
```
___

# Mix and match
Use a combination of different classes for each tier as needed.

```html
<div class="container text-center">
  <!-- Stack the columns on mobile by making one full-width and the other half-width -->
  <div class="row">
    <div class="col-md-8">.col-md-8</div>
    <div class="col-6 col-md-4">.col-6 .col-md-4</div>
  </div>

  <!-- Columns start at 50% wide on mobile and bump up to 33.3% wide on desktop -->
  <div class="row">
    <div class="col-6 col-md-4">.col-6 .col-md-4</div>
    <div class="col-6 col-md-4">.col-6 .col-md-4</div>
    <div class="col-6 col-md-4">.col-6 .col-md-4</div>
  </div>

  <!-- Columns are always 50% wide, on mobile and desktop -->
  <div class="row">
    <div class="col-6">.col-6</div>
    <div class="col-6">.col-6</div>
  </div>
</div>
```
* notice `md` is the breakpoint for mobile
___

# Nesting
simply add a new set of `row` class and `col` classes within an existing `col` class

```html
<div class="container" text-center>
	<div class="row">
		<div class="col-3">Level 1: .col-3</div>
		<div class="col-9">
			<div class="col">Level 2: col</div>
			<div class="col">Level 2: col</div>
		</div>
	</div>
</div>
```
___
