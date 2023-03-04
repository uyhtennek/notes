[Gutters · Bootstrap v5.2 (getbootstrap.com)](https://getbootstrap.com/docs/5.2/layout/gutters/)
___

# Gutters

> [!quote]
> Gutters are the padding between your columns, used to responsively space and align content in the Bootstrap grid system.

* Gutters start at `1.5rem` (`24px`) wide
	* allow us to match our grid to the Bootstrap's `$spacer` scale
* Gutters can be responsively adjusted

use `.gx-*` classes to control the horizontal gutter widths
use `.gy-*` classes to control the vertical gutter widths within a row when columns wrap to new lines
use `g-*` classes to control both horizonal and vertical gutter widths

```html
<div class="container text-center">
	<div class="row gx-2">
		<div class="col-6">Column 1</div>
		<div class="col-6">Column 2</div>
		<div class="col-6">Column 3</div>
	</div>
</div>
```

Responsive variations also exist
* `gx-{breakpoint}-*`
* `gy-{breakpoint}-*`
* `g-{breakpoint}-*`
___

# No gutters

Use `g-0` to remove gutters in Bootstrap's predefined grid classes.
* it removes the negative `margin` from `row` and the horizontal `padding` from all direct children `col`

```html
<div class="row g-0 text-center">
	<div class="col-sm-6 col-md-8">.col-sm-6 .col-md-8</div>
	<div class="col-6 col-md-4">.col-6 .col-md-4</div>
</div>
```
___
