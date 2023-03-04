[Flex · Bootstrap v5.2 (getbootstrap.com)](https://getbootstrap.com/docs/5.2/utilities/flex/)
___

# Enable flex behaviors

> [!quote]
> Apply `display` utilities to create a flexbox container and transform direct children elements into flex items.
* Flex containers and items are able to be modified further with additional flex properties.

```html
<div class="d-flex">I'm a flexbox container!</div>
<div class="d-inline-flex">I'm an inline flexbox container!</div>
```

Responsive variations also exist
* `d-{breakpoint}-flex`
* `d-{breakpoint}-inline-flex`
___

# Direction
Set the direction of flex items in a flex container with direction utilities.

* `flew-row` to set a horizontal left-to-right direction (the browser default)
* `flew-row-reverse` to start the horizontal direction from the opposite side
```html
<div class="d-flex flex-row">
	<div>Flex item 1</div>
	<div>Flex item 2</div>
	<div>Flex item 3</div>
</div>
<div class="d-flex flew-row-reverse">
	<div>Flex item 1</div>
	<div>Flex item 2</div>
	<div>Flex item 3</div>
</div>
```

* `flex-column` to set a vertical direction, top-to-bottom
* `flex-column-reverse` to start the vertical direction from the opposite side
```html
<div class="d-flex flex-column">
	<div>Flex item 1</div>
	<div>Flex item 2</div>
	<div>Flex item 3</div>
</div>
<div class="d-flex flex-column-reverse">
	<div>Flex item 1</div>
	<div>Flex item 2</div>
	<div>Flex item 3</div>
</div>
```

Responsive variations also exist
* `flex-{breakpoint}-{direction}`
* `{direction}` is one of `row`, `row-reverse`, `column`, `column-reverse`
___

# Justify content

Use `justify-content` utilities on flexbox containers to change the alignment of flex items on the main axis.
* x-axis if `flex-direction: row` or class is `flex-row`
	* or `flex-direction: row-reverse` or class is `flex-row-reverse`
* y-axis if `flex-direction: column` or class is `flex-column`
	* or `flex_direction: column-reverse` or class is `flex-column-reverse`

Choose from `start` (browser default), `end`, `center`, `between`, `around`, or `evenly`
* see in [[CSS Flexbox#Alignment and justification between items|CSS Flexbox # Justification]] if you don't understand the options

```html
<div class="d-flex justify-content-start">...</div>
```

Responsive variations also exist
* `justify-content-{breakpoint}-{option}`
___

# Align items

similarly we have `align-items` utilities on flexbox containers to change the alignment of flex items on the cross axis
* y-axis if `flex-direction: row` or `flex-direction: row-reverse`
* x-axis if `flex-direction: column` or `flex-direction: column-reverse`

Choose from `start`, `end`, `center`, `baseline`, or `stretch` (browser default)
* see in [[CSS Flexbox#Alignment and justification between items|CSS Flexbox # Alignment]] if you don't understand the options

```html
<div class="d-flex align-items-stretch">
	<div>Flex item</div>
	<div>Flex item</div>
	<div>Flex item</div>
</div>
```

Responsive variations also exist
* `align-items-{breakpoint}-{option}`
___

# Align self

Use `align-self` utilities on flexbox items to individually change their alignment on the cross axis.
* y-axis if `flex-direction: row` or `row-reverse`
* x-axis if `flex-direction: column` or `column-reverse`

Choose from the same options as [[#Align items]]
* `start`, `end`, `center`, `baseline`, or `stretch` (browser default)

```html
<div class="d-flex">
	<div class="align-self-start">Aligned flex item</div>
</div>
```

Responsive variations also exist
* `align-self-{breakpoint}-{option}`
___

# Fill

Use `flex-fill` class on a series of sibling elements to force them to have the same amount of left over space that is not taken by their content.
* while taking up all available horizontal space

```html
<div class="d-flex">
	<div class="flex-fill">Flex item with a lot of content</div>
	<div class="flex-fill">Flex item</div>
	<div class="flex-fill">Flex item</div>
</div>
```

Responsive variations also exist
* `flex-{breakpoint}-fill`
___

# Grow and shrink

Use `flex-grow-*` utilities to allow a flex item grows to fill available space.
* `*` is `0` or `1`

```html
<div class="d-flex">
	<div class="flex-grow-1">Flex item</div>
	<div>Flex item</div>
	<div>Flex item</div>
</div>
```
* in this example the first element is allowed to use all available space it can, but the remaining two flex items just take their necessary space

Use `flex-shrink-*` utilities to make a flex item shrinks if necessary.
* `*` is `0` or `1`

```html
<div class="d-flex">
	<div class="w-100">Flex item</div>
	<div class="flex-shrink-1">Flex item</div>
</div>
```
* in this example the shrinked element is forced to wrap its contents to a new line, allowing the previous flex item with `w-100` to take more space

Responsive variations also exist
* `flex-{breakpoint}-grow-{0 or 1}`
* `flex-{breakpoint}-shrink-{0 or 1}`
___

# Auto margins
see in [[Bootstrap Utilities - Spacing]]

Here are 2 examples
* `me-auto` pushes the following items to the end
* `ms-auto` pushes the item and its following items to the end
* shorts for *margin from end* or *margin from start*, maybe?

```html
<!-- default: no auto margin -->
<div class="d-flex">
	<div>Flex item</div>
	<div>Flex item</div>
	<div>Flex item</div>
</div>

<!-- push items to the right -->
<div class="d-flex">
	<div class="me-auto">Flex item</div>
	<div>Flex item</div>
	<div>Flex item</div>
</div>

<!-- push items to the left -->
<div class="d-flex">
	<div>Flex item</div>
	<div>Flex item</div>
	<div class="ms-auto">Flex item</div>
</div>
```

We can also do vertical auto margin, but it requires `flex-direction: column`
* we would use `mb-auto` or `mt-auto` in this case
```html
<!-- push items to the bottom -->
<div class="d-flex flex-column" style="height: 200px;">
	<div class="mb-auto">Flex item</div>
	<div>Flex item</div>
	<div>Flex item</div>
</div>

<!-- push items to the top -->
<div class="d-flex flex-column" style="height: 200px;">
	<div>Flex item</div>
	<div>Flex item</div>
	<div class="mt-auto">Flex item</div>
</div>
```
___

# Wrap

We can chnage how flex items wrap in a flex container.
Choose from `nowrap`, `wrap`, or `wrap-reverse`

* `flex-nowrap` (the browser default)
	* flex items that surpass the flex container size would just overflow
* `flex-wrap`
	* normal wrap behavior, wrapping to a new line if there is not enough room
* `flex-wrap-reverse`
	* wrapping to a upper line instead of a bottom line

```html
<div class="d-flex flex-nowrap">
	...
</div>
```

Responsive variations also exist
* `flex-{breakpoint}-{option}`
___

# Order

Use the `order` utilities to change the visual order of specific flex items
* supports for `1` through `5`
	* items with no `order` utilties are `order: 0`
	* add custom CSS for any additional values needed
* also have a `order-first` and a `order-last`
	* which are equivalent to `order: -1` and `order: 6`, respectively
* visual order starts from `-1` and then `0` and `1` and so forth

```html
<div class="d-flex">
	<div class="order-3">First flex item in DOM</div>
	<div class="order-2">Second flex item in DOM</div>
	<div class="order-1">Third flex item in DOM</div>
</div>
```

Responsive variation also exist
* `order-{breakpoint}-{0 to 5}`
* `order-{breakpoint}-{first|last}`
___

# Align content

Use `align-content` utilities on flexbox containers to align flex items together on the cross axis.
* require **more than one row** of flex items
	* either from separated `row` classes
	* or one `row` with `flex-wrap: wrap` or `flex-wrap: wrap-reverse`
* it's like the row version of [[#Justify content]]
	* the effect applys on rows, instead of flex items

Choose from `start` (browser default), `end`, `center`, `between`, `around`, `stretch`

```html
<div class="d-flex align-content-start flex-wrap">
	<div>Flex item</div>
	<div>Flex item</div>
	...
</div>
```

Responsive variations also exist
* `align-content-{breakpoint}-{option}`
___
