[Ruby 101](https://jekyllrb.com/docs/ruby-101/)
* Ruby is a programming language
___

# Gems
= ruby codes / scripts

Jekyll is a gem
* many Jekyll plugins are also gems
___

# Gemfile
a list of gems used in a Ruby project

Every Jekyll site has a `Gemfile` in the main folder, eg.
```ruby
source "https://rubygems.org"

gem "jekyll"

group :jekyll_plugins do
	gem "jekyll-feed"
	gem "jekyll-seo-tag"
end
```
___

# Bundler
a gem that installs all gem in our `Gemfile`

* ensures we're running the same version of Jekyll and its plugins across different environments
* install Bundler using `gem install bundler`
	* only need to install it once, not every time we create a new Jekyll project

to install gems in the `Gemfile`, run the commands in the `Gemfile`'s directory
```bash
bundle install
bundle exec jekyll serve
```
___
