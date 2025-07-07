# ronron.co

This repository contains the source code for **ronron.co**, a simple web site built with [Jekyll](https://jekyllrb.com/).

## Codebase overview

The project follows standard Jekyll conventions and has a minimal structure.

### Core configuration
- `_config.yml` defines site settings. Posts use the `project` layout by default and the site URL is `ronron.co`.
- The `Gemfile` lists `jekyll` and `github-pages` as dependencies.

### Layouts and includes
- Layouts in `_layouts/` provide page templates:
  - `default.html` wraps pages with a header and content section.
  - `home.html` lists all posts and shows each post's cover image.
  - `project.html` displays a post's cover image and any additional images found in `assets/images/posts/`.
- `_includes/` contains snippets such as `head.liquid` for metadata and styles and `header.liquid` for the logo and navigation link.

### Content
- Pages like `index.md` and `info.md` specify a layout and title in their front matter.
- Posts under `_posts/` mostly contain only a title; their content comes from images in `assets/images/posts/<PostTitle>/`.
- The home page (`index.md`) uses the `home` layout to display covers of all posts.

### Styling
- `assets/css/style.scss` imports `main.scss`, which pulls in several Sass partials from `_sass/` to define the theme, layout, typography, and components.
- `_sass/_theme.scss` provides theme settings such as background color and hover mixins.

### Deployment
- A GitHub Actions workflow (`.github/workflows/deployment-pipeline.yml`) builds the site with `jekyll-build-pages` and deploys it to GitHub Pages whenever the `main` branch changes.

## Local development

1. Install Bundler if you don't have it:
   ```bash
   gem install bundler
   ```
2. Install the project dependencies:
   ```bash
   bundle install
   ```
3. Serve the site locally:
   ```bash
   bundle exec jekyll serve
   ```

The site will be available at `http://localhost:4000` by default.
