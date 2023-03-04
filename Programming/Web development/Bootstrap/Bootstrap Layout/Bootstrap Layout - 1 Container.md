Hierachy of Bootstrap's grid goes from
container > [[Bootstrap Layout - 2 Grid Examples|row]] > [[Bootstrap Layout - 3 Column Examples|columns]] > our content
___

# Containers
[Containers · Bootstrap v5.2](https://getbootstrap.com/docs/5.2/layout/containers/)

> [!quote]
> Containers are a fundamental building block of Bootstrap that contain, pad, and align your content within a given device or viewport.

* most basic layout element in Bootstrap
* **required** when using Bootstrap's default grid system
* containers can be nested, although most layouts don't require a nested container

Bootstrap comes with 3 types of containers:
see them in action in [Grid Template # Containers](https://getbootstrap.com/docs/5.2/examples/grid/#containers)

1. `.container` - default container
* which sets a `max-width` at each responsive breakpoint
* its width depends on breakpoints, unless the viewport width is Extra small
* responsive, but fixed-width
* = `.container-sm`

2. `.container-{breakpoint}` - responsive container
* which is `width: 100%` until the specified breakpoint
* its width is the same as viewport width until the viewport width reaches a certain breakpoint
	* if the viewport width keep increasing, its width would remain the same until reached the next breakpoint

3. `.container-fluid` - fluid containers
    which is `width: 100%` at all breakpoints
    its width is always the same as viewport width

| | **Extra small**<br><576px | **Small**<br>&ge;576px | **Medium**<br>&ge;768px | **Large**<br>&ge;992px | **X-Large**<br>&ge;1200px | **XX-Large**<br>&ge;1400px |
| --- | --- | --- | --- | --- | --- | --- |
| `.container` | 100% | 540px | 720px | 960px | 1140px | 1320px |
| `.container-sm` | 100% | 540px | 720px | 960px | 1140px | 1320px |
| `.container-md` | 100% | 100% | 720px | 960px | 1140px | 1320px |
| `.container-lg` | 100% | 100% | 100% | 960px | 1140px | 1320px |
| `.container-xl` | 100% | 100% | 100% | 100% | 1140px | 1320px |
| `.container-xxl` | 100% | 100% | 100% | 100% | 100% | 1320px |
| `.container-fluid` | 100% | 100% | 100% | 100% | 100% | 100% |

* containers are predefined in a Sass map, `_variables.scss`
	* we can customize it if we want to
___
