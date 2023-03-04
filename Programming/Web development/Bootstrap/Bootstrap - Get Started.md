[Bootstrap Docs](https://getbootstrap.com/docs/)
___

# Implementation

1. Prepare the HTML file
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap demo</title>
  </head>
  <body>
    <h1>Hello, world!</h1>
  </body>
</html>
```
* Bootstrap requires the use of the HTML5 doctype
	* must use `<!doctype html>`
* use the `<meta name="viewport">` tag to configure the [[Viewport|responsive havior]] of the page
	* Bootstrap is developed *mobile first*
		* meaning optimize code for mobile devices first and then scale up components as necessary, using CSS media queries
	* To ensure proper rendering and touch zooming for all devices, we need the responsive viewport meta tag

2. **CSS**
* need to `<link>` Bootstrap's stylesheet
```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
```

3. **JavaScript**
* Bootstrap's components used their own JavaScript plugins and [Popper](https://popper.js.org/)
	* [Here](https://getbootstrap.com/docs/5.2/getting-started/introduction/#js-components) is a list of components that require the Bootstrap JavaScript and Popper
* need to enable them
	* they recommended putting them at the end of our pages, right before the closing `</body>`

* Can use either `bootstrap.bundle.js` or `bootstrap.bundle.min.js`
	* both included Popper
```html
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>
```

* alternatively we want Bootstrap and Popper to be separated
	* use either `bootstrap.js` or `bootstrap.min.js`
	* Popper **must** comes first, before Bootstrap's plugins
* Can save some kilobytes by not including Popper
	* if we didn't use dropdowns, popovers, or tooltips
	* to see [which components](https://getbootstrap.com/docs/5.2/getting-started/introduction/#js-components) requires Bootstrap's JavaScript and Popper
```html
<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.10.2/dist/umd/popper.min.js" integrity="sha384-7+zCNj/IqJ95wo16oMtfsKbZ9ccEh31eOz1HGyDuCQ6wgnyJNSYdrPa03rtR1zdB" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.min.js" integrity="sha384-QJHtvGhmr9XOIpI6YVutG+2QOK9T+ZnN4kzFN1RtK3zEFEIsxhlmWl5/YESvpZ13" crossorigin="anonymous"></script>
```

4. **Hello, world!**
* open the page to see if the set-up alright
* then can start building
___

# CDN links

Bootstrap's primary CDN links are
| Description | URL |
| --- | --- |
| CSS | `https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css` |
| JS | `https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js` |

* they used [jsDelivr](https://www.jsdelivr.com/)
___
