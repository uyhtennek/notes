[Basic concepts of flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)
[A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
more deatils in [CSS Flexible Box Layout Module Level 1 (w3.org)](https://www.w3.org/TR/css-flexbox/)
___

# Background

> [!quote]
> The Flexbox Layout (Flexible Box) module aims at providing a more efficient way to lay out, align and distribute space among items in a container, even when their size is unknown and/or dynaic (thus the word "flex").

**Main idea**
* give the container the ability to alter its items' width/height (and order)
	* to best fill the available space
	* to work with all kind of display devices and screen sizes
* expands items to fill available free space or shrinks them to prevent overflow
* flexbox layout is direction-agnostic
	* as opposed to the regular layouts
	* which don't support well when it comes to orientation changing, resizing, stretching, shrinking, etc.

**Note**
* Flexbox layout is most appropriate to the components of an application, and small-scale layouts
	* flexbox deals with layout in one dimension at a time
* Gird layout is intended for larger scale layouts
	* controls columns and rows together
___

# Basics

Flexbox is a whole module and not a single property
* some properties are meant to be set on the container
	* parent element, known as *flew container*
* some are meant to be set on the children
	* known as *flew items*


When working with flexbox we ened to think in terms of two axes
![[flexbox-explained.svg]]

1. the main axis
* defined by the [`flex-direction`](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-direction) property
	* possible values are `row`, `row-reverse`, `column`, `column-reverse`
		* not necessarily horizontal
	* `row` or `row-reverse`, = in *inline direction*; `column` or column-reverse, = in block direction
* the flex items are placed within the container starting from *main-start* and going to *main-end*
	* if `flex-direction` is `row`, it's from left to right
	* if `flex-direction` is `row-reverse`, it's from right to left

2. the cross axis
* the axis that runs perpendicular to `flex-direction`
	* if main axis is `row` or `row-reverse`, cross axis would runs down the columns
		* *cross-start* and *cross-end* can be at the top and bottom of the container or vice versa
	* if main axis is `column` or `column-reverse`, cross axis runs along the rows
		* *cross-start* and *cross-end* can be at the left and right of the container or vice versa


Flexbox makes **no assumption** about the writing mode of the document.
* In the past, CSS was heavily weighted towards horizontal and left-to-right writing modes
* Modern layout methods no longer assume that a line of text will start at the top left and run towards the right-hand side, with new lines appearing one under the other
* notice `flex-direction` is either `row` or `column`, but not like `left`, `right`, `top`, `bottom`

No matter we are writing in English or Arabic, left-to-right or right-to-left, both languages have a horizontal writing mode.
* although direction of main axises are differnt, left-to-right and right-to-left
* the direction of the cross axises are the same
	* start edge of the cross axis is at the top of the flex container
	* end edge at the bottom of the flex container

In the end thinking about start and end rather than left and right becomes natural
Also useful when dealing with other layout methods which follow the same patterns
* eg. CSS Grid Layout
___

# The flex container
An area of a document laid out using flexbox is called a flex container.

1. `display: flex;`
To **create** a flex container,
we set the value of the area's container's [`display`](https://developer.mozilla.org/en-US/docs/Web/CSS/display) property to `flex` or `inline-flex`
* as soon as we do this the container's direct children become flex items
* since CSS defined some initial values for flex items, so they will do the following:
	* Items display in a row (`flex-direction` preperty's default is `row`)
	* Items start from the start edge of the main axis
	* Items do not stretch on the main dimension, but can shrink
	* Items will stretch to fill the size of the cross axis
	* `flex-basis` is set to `auto`
	* `flex-wrap` is set to `nowrap`

The result is
* items will line up in a row
	* size of the content is the size of the item
* If the items' total size exceeds the container's size, they will not wrap but will instead overflow
	* like if each item has a width of 10px, the container has a width of 100px only, but there are 11 items, the last one is still in the same row instead of moving to a next row
* If some items are taller than others, all items will stretch along the cross axis

2. `flex-direction: row;`
To **change the direction** in which our flex items display,
add the `flex-direction` property
* `row`, left-to-right; `row-reverse`, right-to-left;
* `column`, top-to-bottom; `column-reverse`, bottom-to-top

3. `flex-wrap: wrap;`
To **wrap** items,
add the property [`flex-wrap`](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-wrap) with a value of `wrap`
* now items will wrap onto another line if items are too large to all display in one line
* consider each line as a new flex container
	* Any space distribution will happen across that line, without reference to other lines
* `nowrap` is the initial value
	* it makes items shrink to fit the container instead of wrap to a new line
		* since items are using initial flexbox values that allows them to shrink
	* overflow happens when items were not able to shrink, or could not shrink any smaller
* see more in [mastering wrappingflex items](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Mastering_Wrapping_of_Flex_Items)

4. `flex-flow: row wrap;`
We can combine `flex-direction` and `flex-wrap` into [`flex-flow`](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-flow) shorthand
* the first value if `flex-direction`; the second value is `flex-wrap`.
___

# Properties applied to flex items
gain a fuller understanding in [Controlling ratios of flex items along the main axis](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Controlling_Ratios_of_Flex_Items_Along_the_Main_Ax)

Since we are changing the way that available space is distributed amongst flex items by changing the flex items' properties,
it's important to learn the concept of **available space** first

If we have three 100 pixel-wide items in a 500 pixel-wide container,
the space the three items take is 300 pixels, leaving us 200 pixels of available space.

Below is how the items would be laid out if we're using the initial values.
![[flexbox-available-space.png]]

If we instead would like the items to grow and fill the space, then we need to distribute the leftover 200 pixels space between the items.
There are some `flex` properties that we can apply to the flex items to do that.

1. `flex-basis`
* defines the size of an item, in terms of space it leaves as available space
	* the item's base size, to grow and shrink from
* inital value is `auto`, browser looks to see if the item has a size
	* if so, just use the item's `width`, or 100px in the above case, as `flex-basis`
	* if not, use the content's size of the item as `flex-basis`
		* also this is the default behavior of flex items

2. `flex-grow`
* make flex items to grow along the main axis from their `flex-basis`
* cause the item to stretch and take up some more or all available space on that axis
* if we gave all the items a `flex-grow` value of `1`, then the available space in the flex container would be equally shared between the items
	* and they would stretch to fill the container on the main axis
* if we gave the first item a `flew-grow` value of 2 and the others a value of `1` each, 2 parts of the available space will be given to the first item, 1 part each for the others
	* using the above example, 100px out of 200px for item a, and 50px each out of 200px total for item b and c

3. `flex-shrink`
* `flex-grow` deals with adding space to items, but `flex-shrink` controls how space is taken away
* if we don't have enough space in the container to lay out all items, and `flex-shrink` is set to a positive integer, then the items can become smaller than the `flex-basis`
* an item with a high value set for `flex-shrink` will shrink faster than its siblings
* the minimum size of the item will be taken into account when shrinking items
	* so `flex-shrink` may appear less consistent than `flex-grow` in behavior

> [!tip]
> These values for `flex-grow` and `flex-shrink` are proportions.
> Setting item a with `flex: 2 1 200px` and item b and c with `flex: 1 1 200px`,
> is the same as `flex: 20 1 200px` for a and `flex: 10 1 200px` for b and c.

___

# `flex` shorthand

it's rare to use `flex-grow`, `flex-shrink`, `flex-basis` individually, instead they are combined as `flex`
* it allows us to set the values in this order — `flex-grow`, `flex-shrink`, `flex-basis`
* also there are some predefined shorthand values which cover most of the use cases

1. `flex: initial`
* resets the item to the intial values of Flexbox
* it's the same as `flex: 0 1 auto`

2. `flex: auto`
* the same as `flex: 1 1 auto`

3. `flex: none`
* the same as `flex: 0 0 auto`

4. `flex: <positive-number>`
* eg. `flex: 1`, `flex: 2`, and so on
* equals to `flex: 1 1 0` or `flex: 2 1 0` and so on
___

# Alignment and justification between items
The following properties are to be set on the flex container, not on the items.

[`align-items`](https://developer.mozilla.org/en-US/docs/Web/CSS/align-items)
* align the items on the cross axis
* initial value is `stretch`
	* that's why flex items stretch to the height of the flex container by default
	* dictated by the `height` of the tallest item in the container or the `height` of the flex container
* set value to `flex-start` to make the items line up at the start of the flex container
* `flex-end` to align them to the end of container
* `center` to align them in the center

[`justify-content`](https://developer.mozilla.org/en-US/docs/Web/CSS/justify-content)
* align the items on the main axis
	* its direction follows `flex-direction`
* initial value is `flex-start`
	* which lines the items up at the start edge of the container
* set value to `flex-end` to line them up at the end of the container
* `center` to line them up in the center
* set value to `space-between` to evenly share the space between each item
	* first item is at the start of main axis and last item is at the end of main axis
	* every item in between is distributed an equal amount of space
* set value to `space-around` to distribute equal amount of space on the right and left of each item
* `space-evenly` to distribute equal amount of space among items

[`justify-items`](https://developer.mozilla.org/en-US/docs/Web/CSS/justify-items)
* it's ignored in flexbox layouts

visit [Aligning items in a flex container](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Aligning_Items_in_a_Flex_Container) to learn more
___
