# Task: Enhance Jekyll Configuration

## Priority: Medium

## Overview
Optimize Jekyll configuration for better performance, SEO, developer experience, and add useful plugins and settings.

## Files to Modify

### 1. _config.yml
**Current Code (taking into account recent changes):**
```yaml
url: "ronron.co"
baseurl: ""
title: "ronron.co"

permalink: /:year/:month/:day/:title:output_ext

future: true

defaults:
  - scope:
      path: ""
      type: "posts"
    values:
      layout: "project"

exclude:
  - .sass-cache/
  - .jekyll-cache/
  - gemfiles/
  - Gemfile
  - Gemfile.lock
  - node_modules/
  - vendor/bundle/
  - vendor/cache/
  - vendor/gems/
  - vendor/ruby/
  - CNAME
  - docs
```

**Required Change:**
```yaml
# Site settings
title: "ronron.co"
description: "A minimal portfolio showcasing creative projects through visual storytelling"
url: "https://ronron.co"
baseurl: ""
author: "ronron.co"
email: "info@ronron.co"
lang: "en"
timezone: "UTC"

# Build settings
markdown: kramdown
highlighter: rouge
kramdown:
  input: GFM
  hard_wrap: false
  syntax_highlighter: rouge

# Permalink structure
permalink: /:year/:month/:day/:title:output_ext

# Collections
collections:
  projects:
    output: true
    permalink: /:collection/:name/

# Plugins
plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-seo-tag
  - jekyll-redirect-from

# SEO settings
seo:
  type: "Organization"
  name: "ronron.co"
  links:
    - "https://ronron.co"

# Social media
social:
  name: "ronron.co"
  links:
    - "https://ronron.co"

# Feed settings
feed:
  posts_limit: 10

# Future posts
future: true

# Defaults
defaults:
  - scope:
      path: ""
      type: "posts"
    values:
      layout: "project"
      author: "ronron.co"
      permalink: /:year/:month/:day/:title:output_ext
  - scope:
      path: ""
      type: "pages"
    values:
      layout: "default"
      author: "ronron.co"
  - scope:
      path: ""
      type: "projects"
    values:
      layout: "project"
      author: "ronron.co"

# Liquid settings
liquid:
  error_mode: strict
  strict_filters: true
  strict_variables: true

# Performance settings
incremental: true
safe: false
lsi: false

# Sass settings
sass:
  sass_dir: _sass
  style: compressed

# Compression (if using jekyll-compress-html)
compress_html:
  clippings: all
  comments: ["<!-- ", " -->"]
  endings: all
  profile: false
  blanklines: false
  ignore:
    envs: [development]

# Exclude from processing
exclude:
  - .sass-cache/
  - .jekyll-cache/
  - gemfiles/
  - Gemfile
  - Gemfile.lock
  - node_modules/
  - vendor/bundle/
  - vendor/cache/
  - vendor/gems/
  - vendor/ruby/
  - CNAME
  - docs/
  - README.md
  - CLAUDE.md
  - "*.log"
  - .git/
  - .gitignore
  - .DS_Store
  - Thumbs.db

# Keep files
keep_files:
  - .git
  - .svn

# Serving (development only)
host: 0.0.0.0
port: 4000
livereload: true
```

### 2. Gemfile
**Current Code (lines 1-5):**
```ruby
source 'https://rubygems.org'

gem 'jekyll'
gem "github-pages", "~> 232", group: :jekyll_plugins
```

**Required Change:**
```ruby
source 'https://rubygems.org'

gem 'jekyll', '~> 4.3'
gem 'github-pages', '~> 232', group: :jekyll_plugins

# Jekyll plugins
group :jekyll_plugins do
  gem 'jekyll-feed'
  gem 'jekyll-sitemap'
  gem 'jekyll-seo-tag'
  gem 'jekyll-redirect-from'
  gem 'jekyll-compress-html'
  gem 'jekyll-responsive-image'
end

# Development dependencies
group :development do
  gem 'webrick'
  gem 'html-proofer'
end
```

### 3. Create _data/navigation.yml
**New file:**
```yaml
# Navigation structure
main:
  - title: "Home"
    url: "/"
  - title: "Info"
    url: "/info/"

# Social links (if needed)
social:
  - title: "Email"
    url: "mailto:info@ronron.co"
    icon: "email"
```

### 4. Create _data/site.yml
**New file:**
```yaml
# Site metadata
name: "ronron.co"
tagline: "Visual storytelling through creative projects"
keywords: "portfolio, design, creative, projects, art"

# Contact information
contact:
  email: "info@ronron.co"
  
# Analytics (if needed)
analytics:
  google_analytics: ""
  
# Social media (if needed)
social:
  twitter: ""
  instagram: ""
  linkedin: ""

# Project categories
project_categories:
  - furniture
  - design
  - craftsmanship
  - woodworking
```

### 5. Create .jekyll-metadata (gitignore entry)
**Add to .gitignore:**
```
# Jekyll
.jekyll-metadata
.jekyll-cache/
_site/
.sass-cache/

# OS
.DS_Store
Thumbs.db

# Logs
*.log

# Dependencies
node_modules/
vendor/
```

### 6. Create _includes/analytics.liquid (if needed)
**New file:**
```liquid
<!-- Analytics include -->
{% if site.analytics.google_analytics and jekyll.environment == 'production' %}
  <!-- Google Analytics -->
  <script async src="https://www.googletagmanager.com/gtag/js?id={{ site.analytics.google_analytics }}"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', '{{ site.analytics.google_analytics }}');
  </script>
{% endif %}
```

### 7. Update _includes/head.liquid
**Add before closing `</head>` tag:**
```liquid
<!-- SEO Tag -->
{% seo %}

<!-- Feed -->
{% feed_meta %}

<!-- Analytics -->
{% include analytics.liquid %}
```

## Implementation Steps

1. **Update _config.yml**:
   - Add comprehensive site settings
   - Configure plugins and collections
   - Set up performance optimizations
   - Configure Sass compression

2. **Update Gemfile**:
   - Add useful Jekyll plugins
   - Include development dependencies
   - Organize gems into groups

3. **Create Data Files**:
   - Add `_data/navigation.yml` for menu structure
   - Add `_data/site.yml` for site metadata
   - Create structured data for easy maintenance

4. **Update Includes**:
   - Add SEO tag and feed meta to head
   - Create analytics include for future use
   - Ensure proper integration with plugins

5. **Update Gitignore**:
   - Add Jekyll-specific ignores
   - Include OS and log files
   - Exclude build artifacts

## Plugin Benefits

### jekyll-seo-tag
- Automatically generates meta tags
- Structured data markup
- Open Graph and Twitter Card support
- Canonical URLs

### jekyll-sitemap
- Automatic sitemap generation
- Better search engine indexing
- Proper URL structure

### jekyll-feed
- Automatic RSS/Atom feed generation
- RSS discovery in head
- Proper feed formatting

### jekyll-redirect-from
- URL redirection support
- Maintains link equity during moves
- Easy migration handling

### jekyll-compress-html
- Minifies HTML output
- Reduces file sizes
- Improves loading speed

## Testing Considerations

1. **Development Testing**:
   - Test local development server
   - Verify plugin functionality
   - Check build performance
   - Test livereload functionality

2. **Production Testing**:
   - Verify compressed output
   - Test SEO tags generation
   - Check sitemap accessibility
   - Validate feed format

3. **Performance Testing**:
   - Measure build times
   - Check file size reductions
   - Test incremental builds
   - Verify caching behavior

## Benefits

- **Better SEO**: Automatic meta tag generation and structured data
- **Improved Performance**: Sass compression and HTML minification
- **Enhanced Development**: Better error handling and livereload
- **Future-proof**: Extensible configuration for additional features
- **Professional Standards**: Industry-standard Jekyll setup

## Related Tasks

- Should be implemented after meta-information.md
- Supports performance-optimizations.md goals
- Enhances accessibility-enhancements.md features

## Validation Commands

```bash
# Install new dependencies
bundle install

# Test configuration
bundle exec jekyll build --verbose

# Check for errors
bundle exec jekyll doctor

# Serve locally
bundle exec jekyll serve --livereload

# Test HTML output
bundle exec htmlproofer ./_site --check-html --check-opengraph
```

## Notes

- Some plugins may not be supported by GitHub Pages
- Consider using GitHub Actions for full plugin support
- Test thoroughly before deploying to production
- Monitor build times after adding plugins
- Consider adding more plugins as needed (jekyll-archives, jekyll-paginate-v2, etc.)