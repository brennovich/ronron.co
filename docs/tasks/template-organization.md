# Task: Template Organization

## Priority: Low

## Overview
Improve template organization by creating reusable components, extracting repeated logic, and implementing better template structure for maintainability.

## Files to Create/Modify

### 1. Create _includes/page-header.liquid
**New file:**
```liquid
<!-- Reusable page header component -->
<header class="page-header">
  {% if include.title %}
    <h1 class="page-title">{{ include.title }}</h1>
  {% endif %}
  
  {% if include.subtitle %}
    <p class="page-subtitle">{{ include.subtitle }}</p>
  {% endif %}
  
  {% if include.date %}
    <time class="page-date" datetime="{{ include.date | date: '%Y-%m-%d' }}">
      {{ include.date | date: '%B %d, %Y' }}
    </time>
  {% endif %}
  
  {% if include.description %}
    <p class="page-description">{{ include.description }}</p>
  {% endif %}
</header>
```

### 2. Create _includes/image-gallery.liquid
**New file:**
```liquid
<!-- Reusable image gallery component -->
<section class="image-gallery" aria-label="{{ include.label | default: 'Image gallery' }}">
  {% if include.title %}
    <h2 class="gallery-title">{{ include.title }}</h2>
  {% endif %}
  
  {% assign gallery_path = include.path %}
  {% assign cover_image = include.cover | default: 'cover.jpg' %}
  {% assign cover_path = gallery_path | append: cover_image %}
  
  <!-- Cover image -->
  {% if include.show_cover != false %}
    <figure class="gallery-image gallery-cover">
      <img src="{{ cover_path | relative_url }}" 
           alt="{{ include.cover_alt | default: include.title | append: ' - Cover image' }}"
           width="{{ include.width | default: 800 }}" 
           height="{{ include.height | default: 600 }}"
           {% if include.loading %}loading="{{ include.loading }}"{% endif %}>
      {% if include.cover_caption %}
        <figcaption>{{ include.cover_caption }}</figcaption>
      {% endif %}
    </figure>
  {% endif %}
  
  <!-- Additional images -->
  {% assign additional_images = site.static_files | where_exp: "file", "file.path contains gallery_path" | where_exp: "file", "file.path != cover_path" | sort: "path" %}
  {% if additional_images.size > 0 %}
    <div class="gallery-additional">
      {% if include.additional_title %}
        <h3 class="gallery-additional-title">{{ include.additional_title }}</h3>
      {% endif %}
      
      {% for image in additional_images %}
        {% assign image_number = forloop.index %}
        <figure class="gallery-image">
          <img src="{{ image.path | relative_url }}" 
               alt="{{ include.image_alts[image_number] | default: include.title | append: ' - Image ' | append: image_number }}"
               width="{{ include.width | default: 800 }}" 
               height="{{ include.height | default: 600 }}"
               loading="{{ include.loading | default: 'lazy' }}">
          {% if include.image_captions[image_number] %}
            <figcaption>{{ include.image_captions[image_number] }}</figcaption>
          {% endif %}
        </figure>
      {% endfor %}
    </div>
  {% endif %}
</section>
```

### 3. Create _includes/project-card.liquid
**New file:**
```liquid
<!-- Reusable project card component -->
<article class="project-card">
  <a href="{{ include.url | relative_url }}" 
     class="project-card-link"
     aria-label="View {{ include.title }} project">
    
    <div class="project-card-image">
      <img src="{{ include.image | relative_url }}" 
           alt="{{ include.alt | default: include.title | append: ' project cover' }}"
           width="{{ include.width | default: 400 }}" 
           height="{{ include.height | default: 300 }}"
           loading="{{ include.loading | default: 'lazy' }}">
    </div>
    
    <div class="project-card-content">
      <h3 class="project-card-title">{{ include.title }}</h3>
      
      {% if include.date %}
        <time class="project-card-date" datetime="{{ include.date | date: '%Y-%m-%d' }}">
          {{ include.date | date: '%B %Y' }}
        </time>
      {% endif %}
      
      {% if include.description %}
        <p class="project-card-description">{{ include.description }}</p>
      {% endif %}
      
      {% if include.tags %}
        <div class="project-card-tags">
          {% for tag in include.tags %}
            <span class="project-tag">{{ tag }}</span>
          {% endfor %}
        </div>
      {% endif %}
    </div>
  </a>
</article>
```

### 4. Create _includes/navigation-links.liquid
**New file:**
```liquid
<!-- Reusable navigation links component -->
<nav class="content-navigation" aria-label="{{ include.label | default: 'Content navigation' }}">
  <div class="nav-links">
    {% if include.previous %}
      <a href="{{ include.previous.url | relative_url }}" 
         class="nav-link nav-link--prev" 
         rel="prev">
        <span class="nav-direction">← Previous</span>
        <span class="nav-title">{{ include.previous.title }}</span>
        {% if include.previous.description %}
          <span class="nav-description">{{ include.previous.description }}</span>
        {% endif %}
      </a>
    {% endif %}
    
    {% if include.next %}
      <a href="{{ include.next.url | relative_url }}" 
         class="nav-link nav-link--next" 
         rel="next">
        <span class="nav-direction">Next →</span>
        <span class="nav-title">{{ include.next.title }}</span>
        {% if include.next.description %}
          <span class="nav-description">{{ include.next.description }}</span>
        {% endif %}
      </a>
    {% endif %}
  </div>
  
  {% if include.show_all_link %}
    <div class="nav-all">
      <a href="{{ include.all_link | default: '/' | relative_url }}" class="nav-link nav-link--all">
        {{ include.all_text | default: 'View all projects' }}
      </a>
    </div>
  {% endif %}
</nav>
```

### 5. Create _includes/meta-tags.liquid
**New file:**
```liquid
<!-- Reusable meta tags component -->
{% assign page_title = include.title | default: page.title %}
{% assign page_description = include.description | default: page.description | default: page.excerpt | default: site.description %}
{% assign page_image = include.image | default: page.image %}
{% assign page_type = include.type | default: page.type | default: 'website' %}

<!-- Basic meta tags -->
<meta name="description" content="{{ page_description | strip_html | normalize_whitespace | truncate: 160 }}">
<meta name="author" content="{{ include.author | default: page.author | default: site.author }}">

<!-- Open Graph tags -->
<meta property="og:title" content="{{ page_title }}">
<meta property="og:description" content="{{ page_description | strip_html | normalize_whitespace | truncate: 160 }}">
<meta property="og:type" content="{{ page_type }}">
<meta property="og:url" content="{{ page.url | absolute_url }}">
<meta property="og:site_name" content="{{ site.title }}">

{% if page_image %}
  <meta property="og:image" content="{{ page_image | absolute_url }}">
  <meta property="og:image:alt" content="{{ include.image_alt | default: page_title }}">
{% endif %}

<!-- Twitter Card tags -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="{{ page_title }}">
<meta name="twitter:description" content="{{ page_description | strip_html | normalize_whitespace | truncate: 160 }}">

{% if page_image %}
  <meta name="twitter:image" content="{{ page_image | absolute_url }}">
  <meta name="twitter:image:alt" content="{{ include.image_alt | default: page_title }}">
{% endif %}

<!-- Article-specific meta tags -->
{% if page_type == 'article' %}
  <meta property="article:author" content="{{ include.author | default: page.author | default: site.author }}">
  {% if page.date %}
    <meta property="article:published_time" content="{{ page.date | date: '%Y-%m-%d' }}">
  {% endif %}
  {% if page.modified_date %}
    <meta property="article:modified_time" content="{{ page.modified_date | date: '%Y-%m-%d' }}">
  {% endif %}
  {% if page.tags %}
    {% for tag in page.tags %}
      <meta property="article:tag" content="{{ tag }}">
    {% endfor %}
  {% endif %}
{% endif %}
```

### 6. Update _layouts/home.html
**Current Code:**
```html
<!DOCTYPE html>
<html lang="{{ page.lang | default: site.lang | default: "de" }}">
  {%- include head.liquid -%}
  <body>
    {%- include header.liquid -%}

    <section class="content">
      {%- for post in site.posts -%}
        <a href="{{ post.url | relative_url }}">
          <img src="{{ '/assets/images/posts/' | append: post.title | append: '/cover.jpg' | relative_url }}" alt="{{ post.title }}">
        </a>
      {%- endfor -%}
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
    
    {%- include header.liquid -%}
    
    <main id="main-content" class="content" tabindex="-1">
      {% include page-header.liquid 
         title=page.title 
         description=page.description %}
      
      <section class="projects-grid" aria-label="Project portfolio">
        {%- for post in site.posts -%}
          {% include project-card.liquid 
             title=post.title 
             description=post.description 
             url=post.url 
             image=post.cover_image 
             alt=post.cover_alt 
             date=post.date 
             tags=post.tags %}
        {%- endfor -%}
      </section>
    </main>
  </body>
</html>
```

### 7. Update _layouts/project.html
**Current Code:**
```html
<!DOCTYPE html>
<html lang="{{ page.lang | default: site.lang | default: "de" }}">
  {%- include head.liquid -%}
  <body>
    {%- include header.liquid -%}

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
    
    {%- include header.liquid -%}
    
    <main id="main-content" class="content" tabindex="-1">
      <article class="project">
        {% include page-header.liquid 
           title=page.title 
           subtitle=page.subtitle 
           description=page.description 
           date=page.date %}
        
        {% include image-gallery.liquid 
           path=page.image_path 
           title=page.gallery_title 
           cover_alt=page.cover_alt 
           cover_caption=page.cover_caption 
           image_alts=page.image_alts 
           image_captions=page.image_captions 
           loading='lazy' %}
        
        {% if page.content != "" %}
          <section class="project-content">
            {{ content }}
          </section>
        {% endif %}
        
        {% include navigation-links.liquid 
           previous=page.previous 
           next=page.next 
           show_all_link=true 
           all_text='← Back to projects' %}
      </article>
    </main>
  </body>
</html>
```

### 8. Update _includes/head.liquid
**Current Code:**
```html
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta charset="utf-8">

  <title>{{ page.title }} - {{ site.title }}</title>

  <link rel="stylesheet" href="{{ "/assets/css/style.css" | relative_url }}">
  
  <!-- ... other head content ... -->
</head>
```

**Required Change:**
```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  
  <!-- Page Title -->
  <title>{% if page.title %}{{ page.title }} - {{ site.title }}{% else %}{{ site.title }}{% endif %}</title>
  
  <!-- Meta tags -->
  {% include meta-tags.liquid %}
  
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
  
  <!-- SEO plugins -->
  {% seo %}
  {% feed_meta %}
</head>
```

### 9. Create _includes/project-filters.liquid (optional)
**New file:**
```liquid
<!-- Project filtering component -->
<div class="project-filters">
  <h2 class="filters-title">Filter Projects</h2>
  
  <div class="filter-buttons">
    <button class="filter-btn filter-btn--active" data-filter="all">
      All Projects
    </button>
    
    {% assign all_tags = site.posts | map: 'tags' | join: ',' | split: ',' | uniq | sort %}
    {% for tag in all_tags %}
      {% unless tag == blank %}
        <button class="filter-btn" data-filter="{{ tag | slugify }}">
          {{ tag | capitalize }}
        </button>
      {% endunless %}
    {% endfor %}
  </div>
</div>
```

## CSS Updates Required

### Add to _sass/components/_project-card.scss
**New file:**
```scss
.project-card {
  background: $color-white;
  border-radius: $border-radius-lg;
  overflow: hidden;
  box-shadow: $shadow-sm;
  transition: all $transition-base;
  
  &:hover {
    box-shadow: $shadow-md;
    transform: translateY(-2px);
  }
}

.project-card-link {
  display: block;
  text-decoration: none;
  color: inherit;
  
  &:focus-visible {
    @include focus-visible;
  }
}

.project-card-image {
  aspect-ratio: 4/3;
  overflow: hidden;
  
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform $transition-base;
  }
}

.project-card:hover .project-card-image img {
  transform: scale(1.05);
}

.project-card-content {
  padding: $spacing-md;
}

.project-card-title {
  margin: 0 0 $spacing-sm 0;
  font-size: $font-size-lg;
  font-weight: bold;
}

.project-card-date {
  display: block;
  font-size: $font-size-sm;
  color: $color-text-muted;
  margin-bottom: $spacing-sm;
}

.project-card-description {
  font-size: $font-size-sm;
  color: $color-text-light;
  margin-bottom: $spacing-sm;
}

.project-card-tags {
  display: flex;
  flex-wrap: wrap;
  gap: $spacing-xs;
}

.project-tag {
  background: $color-primary;
  color: $color-white;
  padding: $spacing-xs $spacing-sm;
  border-radius: $border-radius-sm;
  font-size: $font-size-xs;
  text-transform: uppercase;
  font-weight: bold;
}
```

## Implementation Steps

1. **Create Include Components**:
   - Add reusable components for common patterns
   - Create flexible, parameterized includes
   - Implement consistent naming conventions

2. **Update Layouts**:
   - Refactor layouts to use new includes
   - Remove duplicated code
   - Implement consistent structure

3. **Add Component Styles**:
   - Create component-specific SCSS files
   - Implement responsive design patterns
   - Add hover and focus states

4. **Test Components**:
   - Verify all includes work correctly
   - Test parameter passing
   - Check responsive behavior

## Benefits

- **Better Maintainability**: Reusable components reduce code duplication
- **Consistency**: Standardized components ensure consistent UI
- **Flexibility**: Parameterized includes allow customization
- **Scalability**: Easy to add new features and pages
- **Developer Experience**: Cleaner, more organized codebase

## Related Tasks

- Should be implemented after semantic-html-structure.md
- Works well with css-scss-structure.md improvements
- Complements accessibility-enhancements.md features

## Testing Considerations

1. **Component Testing**:
   - Test each include in isolation
   - Verify parameter handling
   - Check default values work correctly

2. **Integration Testing**:
   - Test components work together
   - Verify no conflicts between includes
   - Check responsive behavior

3. **Performance Testing**:
   - Measure build time impact
   - Check file size changes
   - Verify no unnecessary includes

## Validation Commands

```bash
# Build and test
bundle exec jekyll build --verbose

# Check for liquid errors
bundle exec jekyll doctor

# Test locally
bundle exec jekyll serve
```

## Notes

- Keep includes focused on single responsibilities
- Use semantic parameter names
- Document include parameters for maintainability
- Consider creating a style guide for component usage