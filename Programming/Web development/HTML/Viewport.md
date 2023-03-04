[Viewport meta tag - MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Viewport_meta_tag)
___

# Viewport

> [!quote]
> A viewport represents a polygonal (normally rectangular) area in computer graphics that is currently being viewed.

In web browser terms
* the currently visible part of the document
	* called *visual viewport*
* or the screen if the document is in full screen mode
* content outside the viewport is not visible on screen until scrolled into view

**visual viewport & layout viewport**
* **visual viewport** = portion of the viewport that is current visible
	* excluding on-screen keyboards, areas outside of a pinch-zoom area, or any other on-screen artifact that doesn't scale with the dimensions of a page
* **layout viewport** = all the portion of a web page, all the part of the document

* when the user has pinched-zoomed, the layout viewport remains the same but the visual viewport becamse smaller
	* the rendered document doesn't change in any way
	* while the visual viewport is updated
___

# Mobile's Virtual viewport

Some mobile devices render pages in a virtual window or viewport, which is usually wider than the screen, and then shrink the rendered result down, so users don't need to scroll left and right.

* if a mobile screen has a width of 640px, pages might be rendered with a virtual viewport of 980px, and then it will be shrunk down to fit into the 640px space

* this is a common feature because not all pages are optimized for mobile
* they will break or look bad when rendered at a small viewport width
* a web page's text sizes or paddings look okay on a 980px wide screen, but maybe on a screen with a width of 640px the elements get too close to each other
	* because the text sizes remain the same but the padding get much narrower on a screen with a width of 640px than a screen with a width of 980px

However, this mechanism works not quite well with pages that are optimized for narrow screens using *media queries*
* it's one of the methods to implement repsonsive design on our sites or apps, it basically detect characteristics or parameters of the user's device so that we can do something about it
* if the virtual viewport is 980px, media queries that would take effect in a screen with a width of 640px, 480px, or less, will never be used.
* our media queries become useless

The viewport `<meta>` tag mitigates this problem of virtual viewport on narrow screen devices.
___

# `<meta>` tag

A typical mobile-optimized site contains something like the following, to make the page works well in a large variation of screen sizes and orientations
```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```


`<meta>` tags contain the following properties:

`width`
* controls the width of the viewport
* can be a number or special value
	* `width=600`, number of pixels
	* `width=device-width`, special value
		* = `100vw`, or 100% of the *viewport width* (vw)
* Minimum `1`. Maximum `10000`. Ignore negative values.

`height`
* controls the height of the viewport
* same as `width`
* except the special value is `height=device-height`
	* = `100vh`, 100% of the *viewport height*

`initial-scale`
* controls the zoom level when the page is first loaded
* Minimum `0.1`. Maximum `10`. Default `1`. Ignore negative values.

`minimum-scale`
* controls how much zoom out is allowed
* Minimum `0.1`. Maximum `10`. Default `1`. Ignore negative values.

`maximum-scale`
* controls how much zoom in is allowed
* Minimum `0.1`. Maximum `10`. Default `1`. Ignore negative values.
	* although any value less than 3 fails accessibility

`user-scalable`
* controls whether zoom in and zoom out actions are allowed
* can be `yes` or `no`, alternatively `1` or `0`
* although setting it to `no` is against Web Content Accessibility Guidelines (WCAG)
	* users with visual impairments such as low vision wouldn't see the page well
	* WCAG requires a minimum of 2x scaling. The best practice is to enable a 5x zoom.
___

# Screen density

Smartphones often have small screens with high resolution
* upwards of 1920 x 1080 pixels, or roughly 400 dpi
* this is why pages can be displayed in a smaller physical size
	* pages are zoomed anyway, to zoom out a page, so the page is smaller physically
	* browsers just translates each CSS "pixel" to less hardware pixels

On the opposite side, zooming in a page, or scaling up a page, can cause problems
* for example browsing a page on a high dpi screen
* although the text can still be smooth and crisp, in other word sharp
* the bitmap images' resolution may not keep up to the scale of the browsers
	* scaling up images make them look less sharp, as each image pixel is scaled up
	* same story as viewing an 1K image in a 4K screen

Therefore web developers may want to design images, or whole layouts, at a higher scale, than their final size and then scale down using CSS or viewport properties
___

# Viewport Sizes

For pages that set an initial or maximum scale, the `width` property actually translates into a *minimum* viewport width
* if the layout needs at least 500 pixels of width, we should use the following markup
```html
<meat name="viewport" content="width=500, initial-scale=1">
```

* When the screen is more than 500 pixels of width, the browser will expand the viewport width
	* rather than zoom in
* When the viewport is less than 500 pixels of width, the page gets scaled down

* other properties that can affect viewport size or limit changes in zoom level are
* `minimum-scale`, `maximum-scale`, and `user-scalable`

If you want to know what mobile and tablet devices have which viewport widths or sizes,
here is a list of [mobile and tablet viewport sizes](https://experienceleague.adobe.com/docs/target/using/experiences/vec/mobile-viewports.html)
___
