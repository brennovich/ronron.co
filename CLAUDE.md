# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based static site for ronron.co, a minimal portfolio website showcasing projects through image galleries. The site follows Jekyll conventions and is deployed to GitHub Pages.

## Development Commands

### Initial Setup
```bash
bundle install                  # Install Ruby dependencies
npm install                     # Install Node.js dependencies (Sharp for images)
npm run process-images          # Generate responsive image variants
bundle exec jekyll serve        # Start development server at http://localhost:4000
```

### Common Workflows
- **Clean build**: `bundle exec jekyll clean && bundle exec jekyll build`
- **Process images**: `npm run process-images` (run after adding/updating images)
- **HTML validation**:
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
- **`home.html`**: Lists all posts with cover images using `responsive_image.liquid` include
- **`project.html`**: Displays cover image and dynamically loads additional images from the post's directory using Jekyll's `site.static_files` to filter and sort images, filtering out processed variants

### Image Processing System
Images use a build-time processing pipeline powered by Sharp (Node.js):
- **Source images**: Original `.jpg`/`.jpeg`/`.png` files in `assets/images/posts/`
- **Generated variants**: Four sizes (thumbnail: 300px, small: 640px, medium: 1024px, large: 1920px)
- **Format outputs**: Both WebP and JPEG for each size (8 files per source image)
- **Naming convention**: `{basename}-{size}.{format}` (e.g., `cover-medium.webp`)
- **Responsive serving**: `_includes/responsive_image.liquid` generates `<picture>` elements with:
  - Home context: thumbnail (1x) and small (2x) for retina
  - Project context: medium (1x) and large (2x) for retina
- **Processed images are gitignored** and regenerated on each deployment

### Styling Architecture
- Main styles in `_sass/main.scss` import modular partials:
  - `_reset.scss`, `_theme.scss`, `_layout.scss`, `_typography.scss`, `_header.scss`, `_content.scss`
  - Component styles in `_sass/components/`: `_logo.scss`, `_nav.scss`
- Compiled via `assets/css/style.scss`

### Key Configuration
- Posts default to `project` layout (_config.yml:14)
- German language default (_layouts/default.html:2)
- Permalink structure: `/:year/:month/:day/:title:output_ext`
- Future posts enabled (`future: true`)

### Deployment
GitHub Actions workflow (`.github/workflows/deployment-pipeline.yml`):
1. Install Node.js dependencies (`npm ci`)
2. Process images (`npm run process-images`)
3. Build Jekyll site
4. Validate HTML with html-proofer
5. Deploy to GitHub Pages
Triggered on pushes to `main` branch or manual workflow dispatch.

## Working with Content

### Adding New Projects
1. Create post file in `_posts/` with format `YYYY-MM-DD-title.md`
2. Add front matter with `title` matching the directory name (e.g., `title: "Meenhard"`)
3. Create directory `assets/images/posts/<Title>/` (title must match exactly)
4. Add `cover.jpg` (required) and any additional numbered images (`1.jpg`, `2.jpg`, etc.)
5. Run `npm run process-images` to generate responsive variants
6. Preview with `bundle exec jekyll serve`

### Image Discovery Mechanism
The `project.html` layout (_layouts/project.html:24-35):
- Uses `site.static_files` to find all images in `/assets/images/posts/<page.title>/`
- Filters out processed variants (files containing `-thumbnail.`, `-small.`, `-medium.`, `-large.`)
- Excludes `cover.jpg`
- Sorts remaining images by path (ensuring 1.jpg, 2.jpg, 3.jpg order)
- Don't use jekyll serve with --detach, rely on build inspecting built html and assets instead