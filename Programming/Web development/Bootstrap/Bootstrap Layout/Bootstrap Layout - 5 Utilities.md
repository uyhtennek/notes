[Utilities for layout · Bootstrap v5.2 (getbootstrap.com)](https://getbootstrap.com/docs/5.2/layout/utilities/)
Utility classes for showing, hiding, aligning, and spacing content.
* for faster mobile-friendly and responsive development
___

# Changing `display`
[Display property · Bootstrap v5.2 (getbootstrap.com)](https://getbootstrap.com/docs/5.2/utilities/display/)
* quickly and responsively toggling common values of the `display` property of components
* mix it with Bootstrap's grid system to show or hide content across specific viewports
	* as well as controlling display when printing

the classes are named using the **format**:
* `d-{value}` for `xs`
* `d-{breakpoint}-{value}` for `sm`, `md`, `lg`, `xl`, and `xxl`
	* affect screen widths with the given breakpoint or larger

`{value}` is  one of
* `none, inline, inline-block, block, grid, table, table-cell, table-row, flex, inline-flex`

**Examples**
```html
<div class="d-inline text-bg-primary">d-inline</div>
<div class="d-inline text-bg-dark">d-inline</div>

<div class="d-block text-bg-primary">d-block</div>
<div class="d-block text-bg-dark">d-block</div>
```


**Hiding elements**

> [!quote]
> For faster mobile-friendly development, use responsive display classes for showing and hiding elements by device. Avoid creating entirely different versions of the same site, instead hide elements responsively for each screen size.

To hide elements simply use the `d-none` class
* or one of the `d-{breakpoint}` classes for any responsive screen variation

To show an element only on a given interval of screen sizes, combine a `d-none` class, or a `d-{breakpoint}-none` class, with a `d-{breakpoint}-block` class
* eg. `d-none d-md-block d-xl-none d-xxl-none`, it shows the element only on medium (md) and large (lg) devices
* see some more [examples](https://getbootstrap.com/docs/5.2/utilities/display/#hiding-elements) in a list

```html
<div class="d-lg-none">hide on lg and wider screens</div>
<div class="d-none d-lg-block">hide on screens smaller than lg</div>
```


**Display in print**
same as other `d-*` classes but it becomes `d-print-{value}` classes
* can combine with other `d-*` classes

```html
<div class="d-print-none">Screen Only (Hide on print only)</div>
<div class="d-none d-print-block">Print Only (Hide on screen only)</div>
<div class="d-none d-lg-block d-print-block">
	Hide up to large on screen, but always show on print
</div>
```
___

# Flexbox options

Not every element's `display` has been changed to `display: flex`
* it would add many unnecessary overrides and unexpectedly change key browser behaviors
* although most of Bootstrap's components are built with flexbox enabled

Whenever a component need to have a `display: flex` property, do so with `d-flex`
* or one of the responsive variants, eg. `d-sm-flex`
* we need `d-flex` classes or `display` value to allow the use of Bootstrap [[Bootstrap Utilities - Flex|flexbox utilities]]
___

# Margin and padding

Use [[Bootstrap Utilities - Spacing|spacing utilities]] to control how elements and components are spaced and sized.
* six-level scale, from 0 to 5
* can set differently for different breakpoints
___

# Toggle `visibility`

If we don't want to toggling `display`, but we want to toggle the `visibility` of an element,
we can use visibility utilities.
* difference is that toggling `display` would make an element disappear in the layout
* toggling `visibility` instead would still make an element affect the layout, but visually hidden

* `visible` or `invisible`
```html
<div class="visible">...</div>
<div class="invisible">...</div>
```

visibility utilities do not modify the `display` value at all, and do not affect layout
* `invisible` elements still take up space in the page
* hidden both visually and for assistive technology / screen reader
___
