[Setup | Jekyll](https://jekyllrb.com/docs/step-by-step/01-setup/)
___

# 1. Setup
need to [install Ruby](https://jekyllrb.com/docs/step-by-step/01-setup/) first

install Jekyll and Bundler
```bash
gem install jekyll bundler
```

then we can run `jekyll -v` to see its version to validate the installation
___

# 2. Hello world

1. create a new director and name it whatever, eg. root
* if prefered, initialize a Git repository here

2. create an `index.html`
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Home</title>
	</head>
	<body>
		<h1>Hello World!</h1>
	</body>
</html>
```

3. build the site
```bash
jekyll build # or
bundle exec jekyll build
```

4. run a local web server
```bash
jekyll serve
```
* it does `jekyll build` and then runs a local web server at `http://localhost:4000`

* to force the browser to refresh with every change, use `jekyll serve --livereload`
* use the `--host` and `--port` arguments to serve the site at a different URL

> [!tip] Markdown
> Jekyll supports Markdown. So we can use `index.md` and write Markdown syntax in it instead of plain HTML in `index.html`.

___

# 3. Liquid
Jekyll is a templating language, which has three main components:

Objects
* tell Liquid to output predefined variables
* eg. `{{ page.title }}`

Tags
* define the logic and control flow for templates
* eg. `{% if page.show_sidebar %}`, `{% endif %}`

Filters
* change the output of a Liquid object
* eg. `{{ "hi" | capitalize }}`, outputs `Hi` instead of `hi`

eg. `index.html`
```html
---
# Important: This part is a front matter. It tells Jekyll to process Liquid.
---

...
		<h1>{{ "Hello World!" | downcase }}</h1>
...
```
___

## 3.1 Front Matter
it is a snippet of YAML

we can set variables in front matter,
and call them in Liquid using the `page` variable
* front matter is needed if we used Liquid tags

eg. `index.html`
```html
---
title: Home
---
<!doctype html>
<html>
	<head>
		<meta charset="utf-8">
		<title>{{ page.title }}</title>
	</head>
</html>
...
...
```
___

# 4. Layouts
the templating stuff

1. Create a `_layouts` directory in the site's root folder

2. Create a template HTML file, eg. `default.html`
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>{{ page.title }}</title>
	</head>
	<body>
		{{ content }}
	</body>
</html>
```
* note that there is no front matter
* `content` is a special variable
	* returns the rendered content

3. Use the layout, in `index.html`
```html
---
layout: default
title: Home
---
<h1>{{ "Hello World!" | downcase }}</h1>
```
* note that the `page` variable means the front matter of the page
	* so `default.html` is able to render `{{ page.title }}`
	* and `default.html` doesn't have a front matter

ex. if it's a Markdown file, like `about.md`
```md
---
layout: default
title: About
---
# About page

This page tells you a little bit about me.
```
___

# 5. Includes
how to navigate between pages

1. Create a directory and a file, `_includes/navigation.html`
```html
<nav>
	<a href="/">Home</a>
	<a href="/about.html">About</a>
</nav>
```

2. use the include tag to add the navigation to `_layouts/default.html`
```html
...
	<body>
		{% include navigation.html %}
		{{ content }}
	</body>
...
```

3. highlight the current page
```html
<nav>
	<a href="/" {% if page.url == "/" %}style="color: red;"{% endif %}>
		Home
	</a>
	<a href="/about.html" {% if page.url == "/about.html" %}style="color: red;"{% endif %}>
		About
	</a>
</nav>
```
* `page.url` outputs the URL of the current page
___

# 6. Data Files

Jekyll supports loading data from YAML, JSON, and CSV files located in a `_data` directory
* make the site easier to maintain

1. Create a data file, `_data/navigation.yml`
```yml
- name: Home
  link: /
- name: About
  link: /about.html
```
* it is then available at `site.data.navigation`

2. in `_includes/navigation.html`, now we can iterate over the data file instead
```html
<nav>
	{% for item in site.data.navigation %}
		<a href="{{ item.link }}" {% if page.url == item.link %}style="color: red;"{% endif %}>
			{{ item.name }}
		</a>
	{% endfor %}
</nav>
```
___

# 7. Assets
the CSS, JS, images, ....

they are often organized using this structure
```
.
????????? _sass
????????? assets
??? ????????? css
??? ????????? images
??? ????????? js
...
```

1. Create a Sass file, `assets/css/styles.scss`
```scss
---
---
@imoprt "main";
```
* the empty front matter tells Jekyll to process the file
* `@import "main"` tells Sass to look for a file called `main.scss` in the `_sass/` directory

2. Create that `_sass/main.scss`
```scss
.current {
	color: green
}
```

3. reference the stylesheet in the layout, `_layouts/default.html`
```html
...
	<head>
		...
		<link rel="stylesheet" href="/assets/css/styles.css">
	</head>
...
```
* `styles.css` is generated by Jekyll and put into `assets/css/`
___

# 8. Blogging
In Jekyll, blogging is powered by text files only

Blog posts live in a folder called `_posts`
* the filename for posts have a special format
	* `YYYY-MM-DD-TITLE.md`
	* or `YYYY-MM-DD-TITLE.html`

1. Create a post, `_posts/2018-08-20-bananas.md`
```md
---
layout: post
author: jill
---
A banana is an edible fruit ??? botanically a berry ??? produced by several kinds of large herbaceous flowering plants in the genus Musa.

In some countries, bananas used for cooking may be called "plantains", distinguishing them from dessert bananas. The fruit is variable in size, color, and firmness, but is usually elongated and curved, with soft flesh rich in starch covered with a rind, which may be green, yellow, red, purple, or brown when ripe.
```
* `author` is a custom variable
	* it's not required
	* it could have been named otherwise

2. Create the `post` layout, `_layouts/post.html`
```html
---
layout: default
---
<h1>{{ page.title }}</h1>
<p>{{ page.date | date_to_string }} - {{ page.author }}</p>

{{ content }}
```

3. create a page which lists all the posts, `/blog.html`
* Jekymll makes posts available at `sites.posts`
```html
---
layout: default
title: Blog
---
<h1>Latest Posts</h1>

<ul>
	{% for post in site.posts %}
		<li>
			<h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
			{{ post.excerpt }}
		</li>
	{% endfor %}
</ul>
```
* `post.url` is automatically set by Jekyll to the output path of the post
* `post.title` is pulled from the post filename
	* it can be overrideen by setting `title` in front matter
* `post.excerpt` is the first paragraph of content by default

4. add this `blog.html` page to the main navigation
```yml
# _data/navigation.yml
- name: Home
  link: /
- name: About
  link: /about.html
- name: Blog
  link: /blog.html
```
___

# 9. Collections
collections are similar to posts except the content doesn't have to be grouped by date

1. we need to tell Jekyll to set up a collection, by creating a file `_config.yml` in the root
```yml
collections:
  authors:
```

2. we need to restart the server to load the configuration

3. create a directory in the root, `_authors`
* the directory name has a format which is `_<collection_name>`
* inside of which are the items in a collection, which we would call *documents*

4. create a document for each author
* eg. `_authors/jill.md`
```md
---
short_name: jill
name: Jill Smith
position: Chief Editor
---
Jill is an avid fruit grower based in the south of France.
```

5. add a page to list all the authors on the site, `staff.html`
* Jekyll makes the collection available at `site.authors`
```html
---
layout: default
title: Staff
---
<h1>Staff</h1>

<ul>
	{% for author in site.authors %}
		<li>
			<h2>{{ author.name }}</h2>
			<h3>{{ author.position }}</h3>
			<p>{{ author.content | markdownify }}</p>
		</li>
	{% endfor %}
</ul>
```
* use the `markdownify` filter to convert markdown content to HTML
	* this happens automatically when outputting using `{{ content }}` so we can skip this step for the layouts previously

6. also need to add this page to `_data/navigation.yml`
```yml
- name: Home
  link: /
- name: About
  link: /about.html
- name: Blog
  link: /blog.html
- name: Staff
  link: /staff.html
```
___

## 9.1 Document page

by default, documents in collections don't have a page
* we need to tweak the configuration and prepare the page

1. add `output: true` to the collection in `_config.yml`
```yml
collections:
  authors:
    otuput: true
```

2. restart the server for the configuration to take effect

3. we can link to the output page using `author.url`, so add this in `staff.html`
```html
...
	<h2><a href="{{ author.url }}">{{ author.name }}</a></h2>
...
```

4. next, we want the author documents to use an author layout, instead of the default one
* so we will need to create a layout for authors, `_layouts/author.html`
```html
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}
```
___

## 9.2 Front matter defaults

5. now we can configure the author documents to use the `author` layout one by one, but instead we can set all author documents to use the layout by default
* we can achieve this by using front matter defaults, in `_config.yml`
```yml
collections:
  authors:
    output: true

defaults:
  - scope:
      path: ""
      type: "authors"
    values:
      layout: "author"
  - scope:
	  path: ""
	  type: "posts"
	values:
	  layout: "post"
  - scope:
	  path: ""
	values:
	  layout: "default"
```

6. now we can remove `layout` from the front matter of all pages and posts

7. and restart Jekyll server
___

## 9.3 Finishing up

8. List author's posts
```html
<!-- file: _layouts/author.html -->
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}

<h2>Posts</h2>
<ul>
	{% assign filtered_posts = site.posts | where: 'author', page.short_name %}
	{% for post in filtered_posts %}
		<li><a href="{{ post.url }}">{{ post.title }}</a></li>
	{% endfor %}
</ul>
```
* in this case we need to match the author's `short_name` to the post's `author`
* here we are filtering the posts by author, then iterate over this filtered list

9. Link to authors page
```html
<!-- file: _layouts/post.html -->
---
layout: default
---
<h1>{{ page.title }}</h1>

<p>
	{{ page.date | date_to_string }}
	{% assign author = site.authors | where: 'short_name', page.author | first %}
	{% if author %}
		- <a href="{{ author.url }}">{{ author.name }}</a>
	{% endif %}
</p>

{{ content }}
```
___

# 10. Deployment

1. create a `Gemfile` in the root
* it's not required but it's good practice
* to ensure the version of Jekyll and other gems remains consistent across different environments
* it should be called `Gemfile` and should not have any extension
* we're doing this with terminal command lines
	* first create a Gemfile with Bundler
	* then add the `jekyll` gem
```bash
bundle init
bundle add jekyll
```

the file should be like
```
# frozen_string_literal: true
source "https://rubygems.org"

gem "jekyll"
```

Bundler installs the gems and creates a `Gemfile.lock`
* it locks the current gem versions for a future `bundle install`
* we can update the gem versions by running `bundle update` if we want

When using a `Gemfile`, we'll run gems' commands with `bundle exec` prefixed
* eg. `bundle exec jekyll serve`
* to restricts the Ruby environment to only use gems set in our `Gemfile`
___

## 10.1 Plugins

....