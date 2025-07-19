# Task: Accessibility Enhancements

## Priority: Medium

## Overview
Implement comprehensive accessibility features including skip links, ARIA attributes, keyboard navigation, and screen reader optimizations.

## Files to Modify

### 1. _layouts/default.html
**Current Code (lines 1-10):**
```html
<!DOCTYPE html>
<html lang="{{ page.lang | default: site.lang | default: "de" }}">
  {%- include head.liquid -%}
  <body>
    {%- include header.liquid -%}
    <section class="content">
      {{ content }}
    </section>
  </body>
</html>
```

**Required Change:**
```html
<!DOCTYPE html>
<html lang="{{ page.lang | default: site.lang | default: "en" }}">
  {%- include head.liquid -%}
  <body>
    <!-- Skip links -->
    <a href="#main-content" class="skip-link">Skip to main content</a>
    <a href="#nav" class="skip-link">Skip to navigation</a>
    
    {%- include header.liquid -%}
    <main id="main-content" class="content" tabindex="-1">
      {{ content }}
    </main>
    
    <!-- Focus manager script -->
    <script>
      // Handle skip link focus
      document.addEventListener('DOMContentLoaded', function() {
        const skipLinks = document.querySelectorAll('.skip-link');
        skipLinks.forEach(link => {
          link.addEventListener('click', function(e) {
            e.preventDefault();
            const target = document.querySelector(this.getAttribute('href'));
            if (target) {
              target.focus();
              // Ensure focus is visible
              target.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }
          });
        });
      });
    </script>
  </body>
</html>
```

### 2. _includes/header.liquid
**Current Code (lines 1-10):**
```html
<header>
  <h1 class="logo">
    <a rel="author" aria-label="{{ site.title }}" href="{{ "/" | relative_url }}">
      <img src="{{ "/assets/images/logo.svg" | relative_url }}" alt="{{ site.title }}">
    </a>
  </h1>
  <nav class="nav">
    <a href="{{ "/info" | relative_url }}">Info</a>
  </nav>
</header>
```

**Required Change:**
```html
<header role="banner">
  <h1 class="logo">
    <a rel="author" aria-label="Go to {{ site.title }} homepage" href="{{ "/" | relative_url }}">
      <img src="{{ "/assets/images/logo.svg" | relative_url }}" alt="{{ site.title }} logo">
    </a>
  </h1>
  <nav id="nav" class="nav" role="navigation" aria-label="Main navigation">
    <ul class="nav-list">
      <li class="nav-item">
        <a href="{{ "/" | relative_url }}" 
           class="nav-link{% if page.url == "/" %} nav-link--active{% endif %}"
           {% if page.url == "/" %}aria-current="page"{% endif %}>
          Home
        </a>
      </li>
      <li class="nav-item">
        <a href="{{ "/info" | relative_url }}" 
           class="nav-link{% if page.url == "/info/" %} nav-link--active{% endif %}"
           {% if page.url == "/info/" %}aria-current="page"{% endif %}>
          Info
        </a>
      </li>
    </ul>
  </nav>
</header>
```

### 3. _layouts/home.html
**Current Code (lines 7-12):**
```html
<section class="content">
  {%- for post in site.posts -%}
    <a href="{{ post.url | relative_url }}">
      <img src="{{ '/assets/images/posts/' | append: post.title | append: '/cover.jpg' | relative_url }}" alt="{{ post.title }}">
    </a>
  {%- endfor -%}
</section>
```

**Required Change:**
```html
<main id="main-content" class="content" tabindex="-1">
  <h1 class="page-title">{{ page.title }}</h1>
  
  <section class="projects-grid" aria-label="Project portfolio">
    <h2 class="visually-hidden">Project Gallery</h2>
    
    {%- for post in site.posts -%}
      <article class="project-preview">
        <a href="{{ post.url | relative_url }}" 
           class="project-link"
           aria-label="View {{ post.title }} project details">
          <img src="{{ '/assets/images/posts/' | append: post.title | append: '/cover.jpg' | relative_url }}" 
               alt="{{ post.description | default: post.title | append: ' project cover image' }}"
               width="400" 
               height="300"
               loading="lazy">
          <h3 class="project-title">{{ post.title }}</h3>
          <span class="project-date" aria-label="Created on">
            <time datetime="{{ post.date | date: '%Y-%m-%d' }}">
              {{ post.date | date: '%B %Y' }}
            </time>
          </span>
        </a>
      </article>
    {%- endfor -%}
  </section>
</main>
```

### 4. _layouts/project.html
**Current Code (lines 7-20):**
```html
<section class="content">
  {% assign cover_path = '/assets/images/posts/' | append: page.title | append: '/cover.jpg' %}
  <div class="project-image">
    <img src="{{ cover_path | relative_url }}" alt="{{ page.title }} - Cover">
  </div>

  {% assign post_images_path = '/assets/images/posts/' | append: page.title | append: '/' %}
  {% assign numbered_images = site.static_files | where_exp: "file", "file.path contains post_images_path" | where_exp: "file", "file.path != cover_path" | sort: "path" %}
  {% for image in numbered_images %}
    <div class="project-image">
      <img src="{{ image.path | relative_url }}" alt="{{ page.title }} - {{ image.name }}">
    </div>
  {% endfor %}
</section>
```

**Required Change:**
```html
<main id="main-content" class="content" tabindex="-1">
  <article class="project" role="article">
    <header class="project-header">
      <h1 class="project-title">{{ page.title }}</h1>
      {% if page.date %}
        <time class="project-date" datetime="{{ page.date | date: '%Y-%m-%d' }}">
          {{ page.date | date: '%B %Y' }}
        </time>
      {% endif %}
      {% if page.description %}
        <p class="project-description">{{ page.description }}</p>
      {% endif %}
    </header>
    
    <section class="project-gallery" aria-label="Project images">
      {% assign cover_path = '/assets/images/posts/' | append: page.title | append: '/cover.jpg' %}
      <figure class="project-image project-cover">
        <img src="{{ cover_path | relative_url }}" 
             alt="{{ page.description | default: page.title | append: ' - Main project image' }}"
             width="800" 
             height="600">
        {% if page.image_caption %}
          <figcaption>{{ page.image_caption }}</figcaption>
        {% endif %}
      </figure>

      {% assign post_images_path = '/assets/images/posts/' | append: page.title | append: '/' %}
      {% assign numbered_images = site.static_files | where_exp: "file", "file.path contains post_images_path" | where_exp: "file", "file.path != cover_path" | sort: "path" %}
      {% if numbered_images.size > 0 %}
        <section class="project-additional-images" aria-label="Additional project images">
          <h2 class="visually-hidden">Additional Images</h2>
          {% for image in numbered_images %}
            {% assign image_number = forloop.index %}
            <figure class="project-image">
              <img src="{{ image.path | relative_url }}" 
                   alt="{{ page.title }} - {{ page.image_descriptions[image_number] | default: 'Detail view' }} (Image {{ image_number }} of {{ numbered_images.size }})"
                   width="800" 
                   height="600"
                   loading="lazy">
              {% if page.image_captions[image_number] %}
                <figcaption>{{ page.image_captions[image_number] }}</figcaption>
              {% endif %}
            </figure>
          {% endfor %}
        </section>
      {% endif %}
    </section>
    
    <!-- Navigation between projects -->
    <nav class="project-navigation" aria-label="Project navigation">
      <h2 class="visually-hidden">Navigate between projects</h2>
      {% if page.previous %}
        <a href="{{ page.previous.url | relative_url }}" class="project-nav-link project-nav-link--prev" rel="prev">
          <span class="project-nav-direction">← Previous</span>
          <span class="project-nav-title">{{ page.previous.title }}</span>
        </a>
      {% endif %}
      {% if page.next %}
        <a href="{{ page.next.url | relative_url }}" class="project-nav-link project-nav-link--next" rel="next">
          <span class="project-nav-direction">Next →</span>
          <span class="project-nav-title">{{ page.next.title }}</span>
        </a>
      {% endif %}
    </nav>
  </article>
</main>
```

### 5. Create _includes/breadcrumbs.liquid
**New file:**
```liquid
{% unless page.url == "/" %}
<nav class="breadcrumbs" aria-label="Breadcrumb navigation">
  <ol class="breadcrumb-list">
    <li class="breadcrumb-item">
      <a href="{{ "/" | relative_url }}" class="breadcrumb-link">
        <span class="visually-hidden">Go to </span>Home
      </a>
    </li>
    {% if page.layout == "project" %}
      <li class="breadcrumb-item" aria-current="page">
        <span class="breadcrumb-current">{{ page.title }}</span>
      </li>
    {% else %}
      <li class="breadcrumb-item" aria-current="page">
        <span class="breadcrumb-current">{{ page.title }}</span>
      </li>
    {% endif %}
  </ol>
</nav>
{% endunless %}
```

### 6. Update CSS for Accessibility
**Add to _sass/_accessibility.scss (new file):**
```scss
// Skip links
.skip-link {
  position: absolute;
  top: -40px;
  left: 6px;
  background: $color-primary;
  color: $color-white;
  padding: 8px;
  text-decoration: none;
  z-index: $z-index-tooltip;
  border-radius: $border-radius-sm;
  transition: top $transition-fast;
  
  &:focus {
    top: 6px;
  }
}

// Screen reader only content
.visually-hidden {
  @include sr-only;
}

// Focus management
[tabindex="-1"] {
  outline: none;
}

// Focus indicators
a:focus-visible,
button:focus-visible,
input:focus-visible,
textarea:focus-visible,
select:focus-visible {
  @include focus-visible;
}

// Navigation enhancements
.nav-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  gap: $spacing-md;
  
  @include media-max($breakpoint-sm) {
    flex-direction: column;
    gap: $spacing-sm;
  }
}

.nav-item {
  margin: 0;
}

.nav-link {
  display: block;
  padding: $spacing-sm $spacing-md;
  text-decoration: none;
  color: $color-primary;
  border-radius: $border-radius-sm;
  transition: all $transition-fast;
  
  &--active {
    background-color: $color-primary;
    color: $color-white;
  }
  
  &:hover:not(.nav-link--active) {
    @include hover-text;
  }
  
  &:focus-visible {
    @include focus-visible;
  }
}

// Project navigation
.project-navigation {
  margin-top: $spacing-xl;
  padding-top: $spacing-lg;
  border-top: 1px solid $color-text-light;
  display: flex;
  gap: $spacing-md;
  
  @include media-max($breakpoint-sm) {
    flex-direction: column;
  }
}

.project-nav-link {
  display: block;
  padding: $spacing-md;
  text-decoration: none;
  color: $color-text;
  border: 1px solid $color-text-light;
  border-radius: $border-radius-md;
  transition: all $transition-fast;
  
  &--prev {
    margin-right: auto;
  }
  
  &--next {
    margin-left: auto;
  }
  
  &:hover {
    border-color: $color-primary;
    background-color: rgba($color-primary, 0.1);
  }
  
  &:focus-visible {
    @include focus-visible;
  }
}

.project-nav-direction {
  display: block;
  font-size: $font-size-sm;
  color: $color-text-muted;
  margin-bottom: $spacing-xs;
}

.project-nav-title {
  display: block;
  font-weight: bold;
}

// Breadcrumbs
.breadcrumbs {
  margin-bottom: $spacing-md;
}

.breadcrumb-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  gap: $spacing-xs;
  align-items: center;
}

.breadcrumb-item {
  margin: 0;
  
  &:not(:last-child)::after {
    content: "→";
    margin-left: $spacing-xs;
    color: $color-text-muted;
  }
}

.breadcrumb-link {
  text-decoration: none;
  color: $color-primary;
  
  &:hover {
    text-decoration: underline;
  }
  
  &:focus-visible {
    @include focus-visible;
  }
}

.breadcrumb-current {
  color: $color-text;
  font-weight: bold;
}

// High contrast mode support
@media (prefers-contrast: high) {
  .nav-link--active {
    outline: 2px solid $color-text;
  }
  
  .project-nav-link:hover {
    outline: 2px solid $color-primary;
  }
}

// Reduced motion support
@media (prefers-reduced-motion: reduce) {
  .skip-link {
    transition: none;
  }
  
  .nav-link,
  .project-nav-link {
    transition: none;
  }
}
```

### 7. Update _sass/main.scss
**Add import:**
```scss
@import
    'theme',
    'reset',
    'utilities',
    'accessibility',
    'typography',
    'layout',
    'header',
    'components/logo',
    'components/nav';
```

## Implementation Steps

1. **Update Layout Templates**:
   - Add skip links to default layout
   - Implement proper heading hierarchy
   - Add ARIA labels and landmarks

2. **Enhance Navigation**:
   - Add proper navigation structure
   - Implement current page indicators
   - Add keyboard navigation support

3. **Create Accessibility Components**:
   - Add breadcrumb navigation
   - Implement project navigation
   - Create screen reader utilities

4. **Add CSS Enhancements**:
   - Create comprehensive focus indicators
   - Add high contrast mode support
   - Implement reduced motion preferences

5. **Test and Validate**:
   - Test with screen readers
   - Verify keyboard navigation
   - Check accessibility tools compliance

## Testing Considerations

1. **Screen Reader Testing**:
   - Test with NVDA, JAWS, VoiceOver
   - Verify content is announced correctly
   - Check skip links functionality

2. **Keyboard Navigation**:
   - Test tab order
   - Verify all interactive elements are accessible
   - Check focus indicators are visible

3. **Accessibility Tools**:
   - Use aXe DevTools
   - Test with WAVE browser extension
   - Validate with Lighthouse accessibility audit

4. **High Contrast Testing**:
   - Test with Windows High Contrast mode
   - Verify color contrast ratios
   - Check focus indicators visibility

## Benefits

- **WCAG 2.1 AA Compliance**: Meets accessibility standards
- **Better User Experience**: Improved navigation for all users
- **SEO Benefits**: Better structured content helps search engines
- **Legal Compliance**: Reduces accessibility compliance risks
- **Inclusive Design**: Makes content accessible to users with disabilities

## Related Tasks

- Should be implemented after semantic-html-structure.md
- Works with image-accessibility.md for complete coverage
- Complements meta-information.md for full page optimization

## Validation Commands

```bash
# Build and test
bundle exec jekyll serve

# Accessibility testing tools
# Use browser accessibility extensions
# Test with screen readers
# Validate with accessibility auditing tools
```

## Notes

- Test with actual users who rely on assistive technologies
- Consider implementing keyboard shortcuts for power users
- Regular accessibility audits should be part of the development process
- Keep accessibility documentation updated as features are added