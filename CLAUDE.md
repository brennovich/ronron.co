# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based static site for ronron.co, a minimal portfolio website showcasing projects through image galleries. The site follows Jekyll conventions and is deployed to GitHub Pages.

## Development Commands

### Local Development
- Install dependencies: `bundle install`
- Start development server: `bundle exec jekyll serve`
- Site will be available at `http://localhost:4000`

### HTML Validation
The CI pipeline includes HTML validation using html-proofer:
```bash
gem install --no-document --user-install html-proofer
$(ruby -e 'print Gem.user_dir')/bin/htmlproofer ./_site
```

## Architecture Overview

### Content Structure
- **Posts**: Located in `_posts/` with minimal front matter (title only)
- **Images**: Project images stored in `assets/images/posts/<PostTitle>/`
  - `cover.jpg` is the main project image
  - Additional numbered images (1.jpg, 2.jpg, etc.) are displayed in sequence
- **Pages**: `index.md` (home) and `info.md` use front matter to specify layout

### Layout System
- **`default.html`**: Base template with header and content section
- **`home.html`**: Lists all posts with cover images
- **`project.html`**: Displays cover image and dynamically loads additional images from the post's directory using Jekyll's `site.static_files` to filter and sort images

### Styling Architecture
- Main styles in `_sass/main.scss` import modular partials:
  - `_reset.scss`, `_theme.scss`, `_layout.scss`, `_typography.scss`, `_header.scss`
  - Component styles in `_sass/components/`: `_logo.scss`, `_nav.scss`
- Compiled via `assets/css/style.scss`

### Key Configuration
- Posts default to `project` layout (_config.yml:14)
- German language default (_layouts/default.html:2)
- Permalink structure: `/:year/:month/:day/:title:output_ext`
- Future posts enabled (`future: true`)

### Deployment
- GitHub Actions workflow builds and deploys to GitHub Pages
- Includes HTML validation step before deployment
- Triggered on pushes to main branch

## Working with Content

### Adding New Projects
1. Create post file in `_posts/` with format `YYYY-MM-DD-title.md`
2. Add front matter with title matching the directory name
3. Create directory `assets/images/posts/<Title>/`
4. Add `cover.jpg` and any additional numbered images

### Image Handling
The `project.html` layout automatically discovers and displays images from the post's directory, excluding the cover image and sorting by path.