# Task: Add Essential Meta Information

## Priority: High

## Overview
Add crucial meta tags for SEO, accessibility, and social sharing. Fix language settings and implement proper page descriptions.

## Files to Modify

### 1. _includes/head.liquid
**Current Code (lines 1-19):**
```html
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta charset="utf-8">

  <title>{{ page.title }} - {{ site.title }}</title>

  <link rel="stylesheet" href="{{ "/assets/css/style.css" | relative_url }}">

  <link rel="icon" type="image/x-icon" href="{{ "/assets/images/favicon.ico" | relative_url }}">
  <link rel="icon" type="image/png" sizes="16x16" href="{{ "/assets/images/favicon-16x16.png" | relative_url }}">
  <link rel="apple-touch-icon" sizes="180x180" href="{{ "/assets/images/apple-touch-icon.png" | relative_url }}">
  <link rel="icon" type="image/svg+xml" href="{{ "/assets/images/favicon.svg" | relative_url }}">
  <link rel="icon" type="image/png" sizes="512x512" href="{{ "/assets/images/android-chrome-512x512.png" | relative_url }}">

  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black">

  <meta name="theme-color" content="#f6f4f2">
</head>
```

**Required Change:**
```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  
  <!-- Page Title -->
  <title>{% if page.title %}{{ page.title }} - {{ site.title }}{% else %}{{ site.title }}{% endif %}</title>
  
  <!-- Meta Description -->
  <meta name="description" content="{{ page.description | default: page.excerpt | default: site.description | strip_html | normalize_whitespace | truncate: 160 }}">
  
  <!-- Open Graph Tags -->
  <meta property="og:title" content="{% if page.title %}{{ page.title }} - {{ site.title }}{% else %}{{ site.title }}{% endif %}">
  <meta property="og:description" content="{{ page.description | default: page.excerpt | default: site.description | strip_html | normalize_whitespace | truncate: 160 }}">
  <meta property="og:type" content="{% if page.layout == 'project' %}article{% else %}website{% endif %}">
  <meta property="og:url" content="{{ page.url | absolute_url }}">
  <meta property="og:site_name" content="{{ site.title }}">
  {% if page.layout == 'project' %}
    <meta property="og:image" content="{{ '/assets/images/posts/' | append: page.title | append: '/cover.jpg' | absolute_url }}">
    <meta property="og:image:alt" content="{{ page.title }} - Cover image">
  {% endif %}
  
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="{% if page.title %}{{ page.title }} - {{ site.title }}{% else %}{{ site.title }}{% endif %}">
  <meta name="twitter:description" content="{{ page.description | default: page.excerpt | default: site.description | strip_html | normalize_whitespace | truncate: 160 }}">
  {% if page.layout == 'project' %}
    <meta name="twitter:image" content="{{ '/assets/images/posts/' | append: page.title | append: '/cover.jpg' | absolute_url }}">
    <meta name="twitter:image:alt" content="{{ page.title }} - Cover image">
  {% endif %}
  
  <!-- Canonical URL -->
  <link rel="canonical" href="{{ page.url | absolute_url }}">
  
  <!-- Stylesheets -->
  <link rel="stylesheet" href="{{ "/assets/css/style.css" | relative_url }}">
  
  <!-- Favicons -->
  <link rel="icon" type="image/x-icon" href="{{ "/assets/images/favicon.ico" | relative_url }}">
  <link rel="icon" type="image/png" sizes="16x16" href="{{ "/assets/images/favicon-16x16.png" | relative_url }}">
  <link rel="apple-touch-icon" sizes="180x180" href="{{ "/assets/images/apple-touch-icon.png" | relative_url }}">
  <link rel="icon" type="image/svg+xml" href="{{ "/assets/images/favicon.svg" | relative_url }}">
  <link rel="icon" type="image/png" sizes="512x512" href="{{ "/assets/images/android-chrome-512x512.png" | relative_url }}">
  
  <!-- Mobile App Meta -->
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black">
  <meta name="theme-color" content="#f6f4f2">
  
  <!-- Author and Generator -->
  <meta name="author" content="{{ site.author | default: site.title }}">
  <meta name="generator" content="Jekyll {{ jekyll.version }}">
  
  <!-- Robots -->
  <meta name="robots" content="index, follow">
</head>
```

### 2. _layouts/default.html
**Current Code (line 2):**
```html
<html lang="{{ page.lang | default: site.lang | default: "de" }}">
```

**Required Change:**
```html
<html lang="{{ page.lang | default: site.lang | default: "en" }}">
```

**Rationale:** The content appears to be in English, so the default language should be "en" unless the site is actually in German.

### 3. _config.yml
**Current Code (lines 1-28):**
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
```

**Required Change:**
```yaml
url: "https://ronron.co"
baseurl: ""
title: "ronron.co"
description: "A minimal portfolio showcasing creative projects through visual storytelling"
author: "ronron.co"
lang: "en"
timezone: "UTC"

permalink: /:year/:month/:day/:title:output_ext

future: true

defaults:
  - scope:
      path: ""
      type: "posts"
    values:
      layout: "project"
  - scope:
      path: ""
      type: "pages"
    values:
      layout: "default"

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
```

### 4. index.md
**Current Code (lines 1-4):**
```markdown
---
layout: home
title: "Miau"
---
```

**Required Change:**
```markdown
---
layout: home
title: "Miau"
description: "A curated collection of creative projects and visual work"
---
```

### 5. info.md
**Current Code (lines 1-4):**
```markdown
---
layout: default
title: "Info"
---
```

**Required Change:**
```markdown
---
layout: default
title: "Info"
description: "About ronron.co and the creative philosophy behind the projects"
---
```

### 6. Update Post Files
**Add to each post in _posts/ (example for 2025-01-01-hocker.md):**
```markdown
---
title: Hocker
description: "Hocker project - A creative exploration in design and functionality"
---
```

**Apply similar changes to:**
- `_posts/2025-02-01-leonard.md`
- `_posts/2025-03-01-meenhard.md`
- `_posts/2025-04-01-sideboard.md`

## Implementation Steps

1. **Update _config.yml**:
   - Add `https://` to url
   - Add `description`, `author`, `lang`, `timezone` fields
   - Add default layout for pages
   - Add docs/ to exclude list

2. **Update _includes/head.liquid**:
   - Reorder meta tags logically
   - Add comprehensive SEO meta tags
   - Add Open Graph and Twitter Card tags
   - Add canonical URL
   - Add proper conditional logic for project vs page meta

3. **Update _layouts/default.html**:
   - Change default language from "de" to "en"

4. **Update page front matter**:
   - Add descriptions to index.md and info.md
   - Add descriptions to all posts

## Testing Considerations

1. **SEO Testing**:
   - Test with Google Search Console
   - Validate meta tags in browser dev tools
   - Check Open Graph tags with Facebook debugger
   - Test Twitter Card with Twitter validator

2. **Accessibility Testing**:
   - Verify proper title hierarchy
   - Test with screen readers
   - Validate HTML structure

3. **Social Sharing Testing**:
   - Test links on Facebook, Twitter, LinkedIn
   - Verify images appear correctly
   - Check description truncation

## Benefits

- **Improved SEO**: Proper meta descriptions and structured data help search engines understand content
- **Better Social Sharing**: Open Graph and Twitter Card tags improve link previews
- **Enhanced Accessibility**: Proper language attributes help screen readers
- **Professional Appearance**: Consistent meta information across all pages
- **Future-proof**: Extensible structure for additional meta tags

## Related Tasks

- Complements semantic-html-structure.md for complete page structure
- Should be implemented before performance-optimizations.md
- Works with image-accessibility.md for complete meta coverage

## Validation Commands

After implementation, run:
```bash
# Build site locally
bundle exec jekyll serve

# Test meta tags in browser dev tools
# Check HTML validation
# Test social sharing previews
```