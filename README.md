# Joshua Shew's Personal Website

This repository contains the source code for my personal website and blog, built with Jekyll and the Minimal Mistakes theme. The site includes my profile, blog posts, and portfolio.

## Site Structure

- `_posts/`: Contains all blog posts in Markdown format
- `_pages/`: Custom pages like About, Now, etc.
- `_data/`: Configuration files like navigation
- `_config.yml`: Main Jekyll configuration
- `assets/`: Images and other media
- `_posts/example-posts/`: Templates for new blog posts

## How to Post to the Blog

To add a new blog post:

1. Draft the content of the blog post in Obsidian (or your preferred Markdown editor)
2. Copy a blog post template file from `_posts/example-posts`
3. Copy in the contents
4. Preview the the contents with `bundle exec jekyll serve`

## Setup and Development Environment

To set up the development environment:

1. Install `rbenv` for managing Ruby versions
   - Install with Homebrew: `brew install rbenv`
   - Set up `rbenv` in your shell by adding `eval "$(rbenv init -)"` to your `.zshrc` file
2. Install the latest version of Ruby
   - Check available versions with `rbenv install -l`
   - Install with `rbenv install <ver>`
   - Set to be the global version with `rbenv global <ver>`
3. Install Bundler: `gem install bundler`
4. Then finally, `bundle install`

After making sure posts look good, merge and push to deploy the changes.

## Content Features

The site supports various content types and features:

- **Media**: Support for compressing GIFs using FFmpeg and gifsicle
- **Code Snippets**: Syntax highlighting and properly formatted code blocks
- **Tags and Categories**: Organize content by tags and categories
- **Custom Pages**: About, Now, etc.

## Credits

This site is built with:

- [Jekyll](https://jekyllrb.com/)
- [Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/)
- GitHub Pages for hosting

## License

Content is copyrighted by Joshua Shew unless otherwise noted.
