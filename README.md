# Personal Website & Blog

A Jekyll-powered personal website hosted on GitHub Pages.

## Structure

- `_config.yml` - Jekyll configuration
- `_layouts/` - Page layouts (default, post, page)
- `_pages/` - Static pages (index, about, contact, blog)
- `_posts/` - Blog posts in Markdown format
- `styles.css` - Site stylesheet
- `headshot.jpg`, `monogram.png` - Assets

## Local Development

### Prerequisites
- Ruby 3.1+ (required for Jekyll 4)
- Bundler

### Setup
```bash
# Install dependencies (may require updating Ruby version)
bundle install

# Run Jekyll server
bundle exec jekyll serve

# Site will be available at http://localhost:4000
```

**Note**: If you have an older Ruby version (< 3.1), you can either:
1. Use Ruby version manager (rbenv, rvm) to install Ruby 3.1+
2. Skip local development and use GitHub Actions to build the site automatically

## Adding New Blog Posts

1. Create a new Markdown file in `_posts/`
2. Use the filename format: `YYYY-MM-DD-slug.md`
3. Add front matter:
```yaml
---
layout: post
title: "Your Post Title"
date: 2025-01-01
tags: [Tag1, Tag2]
---
```

4. Write your content in Markdown below the front matter

## Deploying to GitHub Pages

This repository is configured for automatic deployment via GitHub Actions:

1. Push your code to the `main` branch
2. GitHub Actions will build and deploy your site
3. Your site will be available at `https://yourusername.github.io/repo-name`

**Note**: Update the `url` in `_config.yml` to match your GitHub Pages URL.

## Features

- **Responsive Design**: Adapts to mobile, tablet, and desktop
- **Dynamic Blog Listing**: Automatically generates post list with tags, dates, and read times
- **Tag Filtering**: Filter posts by tags on the blog page
- **Search**: Client-side search for blog posts
- **Markdown Support**: Write posts in Markdown

## Customization

### Changing the Theme Colors
Edit CSS variables in `styles.css`:
- `--sidebar-bg`: Sidebar background color
- `--brand`: Brand color for name
- `--link`: Link color
- Other design tokens

### Adding Pages
Create new files in `_pages/` with front matter:
```yaml
---
layout: page
title: Your Page Title
permalink: /your-page/
---
```

### Modifying Layouts
Edit files in `_layouts/`:
- `default.html`: Base layout with sidebar
- `page.html`: Standard page layout
- `post.html`: Blog post layout

