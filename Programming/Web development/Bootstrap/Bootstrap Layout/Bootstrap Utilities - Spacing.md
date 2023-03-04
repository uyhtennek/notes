# Spacing utilities Notation

The classes are named using the format
* `{property}{sides}-{size}` for `xs` breakpoint
* `{property}{sides}-{breakpoint}-{size}` for `sm`, `md`, `lg`, `xl`, and `xxl`

`{property}` is one of
* `m` for *margin*
* `p` for *padding*

`{sides}` is one of
* `t` for *top*
* `b` for *bottom*
* `s` for *start*
* `e` for *end*
* `x` for *x-axis*, both left and righ
* `y` for *y-axis*, both top and bottom
* blank, for all 4 sides of the element

`{size}` is one of
* `0`
* `1` = `$spacer * .25`
* `2` = `$spacer * .5`
* `3` = `$spacer`
* `4` = `$spacer * 1.5`
* `5` = `$spacer * 3`
* `auto`, for classes that set the `margin` to auto

the sizes are predefined by Bootstrap, so those are default values
* so we can as well add more sizes by adding entries to the `$spacers` Sass map variable

**Examples**
* `mt-0`
* `ms-1`
* `px-2`
* `p-3`


**Horizontal centering**
We can use `mx-auto` class for horizontally centering fixed-width block level content
* content that has `display: block` and a `width` set

```html
<div class="mx-auto" style="width: 200px;">
	Centered element
</div>
```
___

# Negative margin

To use the negative margins utilities, we first need to enable it in Sass by setting `$enable-negative-margins: true`
* `padding` properties don't have negative values though

Syntax is the same as the default positive margin utilities, except with an `n` before the `{size}`
* eg. `mt-n1`
___

# Gap

When using `display: grid`, we can use the `gap` utilities on the parent grid container to apply margins among its children.
* so we can save on having to add margin utilities to individual grid items
* Gap utilities are responsive by default, and are generated via Bootstrap's utilities API

```html
<div class="d-grid gap-3">
	<div>Grid item 1</div>
	<div>Grid item 2</div>
	<div>Grid item 3</div>
</div>
```
___
