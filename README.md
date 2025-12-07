# ronron.co

Minimal portfolio website showcasing projects through image galleries.

## Development Setup

### Prerequisites

- Ruby (for Jekyll)
- Node.js LTS (for image processing)
- Bundler: `gem install bundler`

### Initial Setup

1. Install Ruby dependencies:
   ```bash
   bundle install
   ```

2. Install Node.js dependencies:
   ```bash
   npm install
   ```

3. Process images:
   ```bash
   npm run process-images
   ```

4. Start Jekyll development server:
   ```bash
   bundle exec jekyll serve
   ```

5. Open http://localhost:4000

### Adding New Projects

1. Create a new post file in `_posts/` with format `YYYY-MM-DD-title.md`
2. Add front matter with title matching the directory name
3. Create directory `assets/images/posts/<Title>/`
4. Add `cover.jpg` and any additional numbered images (1.jpg, 2.jpg, etc.)
5. Run `npm run process-images` to generate responsive image variants
6. Build and preview with `bundle exec jekyll serve`

### Image Processing

Images are processed at build time using Sharp to generate multiple sizes:
- **thumbnail**: 300px (for home page)
- **small**: 640px (for retina home page)
- **medium**: 1024px (for project pages)
- **large**: 1920px (for retina project pages)

Each size is generated in both WebP and JPEG formats for optimal browser support.

Processed images are gitignored and regenerated on each build.

## Deployment

The site deploys automatically to GitHub Pages when changes are pushed to the `main` branch. The GitHub Actions workflow handles image processing, Jekyll build, HTML validation, and deployment.
