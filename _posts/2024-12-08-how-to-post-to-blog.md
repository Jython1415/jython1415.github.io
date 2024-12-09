---
title: "How I Post to My Blog"
categories:
  - Blog
tags:
  - Blogging
  - Programming
---

It had been almost a year since my last post, so I forgot how to update my blog. Oops.

Here's how I did it this time around.

1. Draft the content of the blog post in Obsidian (my preferred Markdown editor)
2. Copy a blog post template file from `_posts/example-posts`
3. Copy in the contents
4. Preview the the contents with `bundle exec jekyll serve`

To run `bundle exec jekyll serve`, I had to install Ruby and set up the project first.

1. Install `rbenv` for managing Ruby versions
	1. Install with Homebrew: `brew install rbenv`
	2. Set up `rbenv` in my shell by adding `eval "$(rbenv init -)"` to my `.zshrc` file
2. Install the latest version of Ruby
	1. Check available versions with `rbenv install -l`
	2. Install with `rbenv install <ver>`
	3. Set to be the global version (unless I have other Ruby environments) with `rbenv global <ver>`
3. Install Bundler: `gem install bundler`
4. Then finally, `bundle install`

After making sure the post looks good, I just merge and push.
