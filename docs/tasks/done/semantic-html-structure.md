# Task: Improve Semantic HTML Structure

## Priority: High

## Overview
Replace generic `<section>` elements with proper semantic HTML5 elements and add proper heading hierarchy to improve accessibility and SEO.

## Files to Modify

### 1. _layouts/default.html
**Current Code (line 6):**
```html
<section class="content">
  {{ content }}
</section>
```

**Required Change:**
```html
<main class="content">
  {{ content }}
</main>
```

**Rationale:** The `<main>` element represents the primary content of the page, which is more semantically correct than a generic `<section>`.

### 2. _layouts/home.html
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
<main class="content">
  <h1>{{ page.title }}</h1>
  <section class="projects-grid">
    {%- for post in site.posts -%}
      <article class="project-preview">
        <a href="{{ post.url | relative_url }}">
          <img src="{{ '/assets/images/posts/' | append: post.title | append: '/cover.jpg' | relative_url }}" alt="{{ post.title }}">
          <h2>{{ post.title }}</h2>
        </a>
      </article>
    {%- endfor -%}
  </section>
</main>
```

**Rationale:** 
- `<main>` for primary content
- `<h1>` for page title
- `<section>` with descriptive class for projects listing
- `<article>` for each project preview (semantically correct for standalone content)
- `<h2>` for project titles to establish heading hierarchy

### 3. _layouts/project.html
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
<main class="content">
  <article class="project">
    <header class="project-header">
      <h1>{{ page.title }}</h1>
    </header>
    
    <section class="project-gallery">
      {% assign cover_path = '/assets/images/posts/' | append: page.title | append: '/cover.jpg' %}
      <figure class="project-image project-cover">
        <img src="{{ cover_path | relative_url }}" alt="{{ page.title }} - Cover">
      </figure>

      {% assign post_images_path = '/assets/images/posts/' | append: page.title | append: '/' %}
      {% assign numbered_images = site.static_files | where_exp: "file", "file.path contains post_images_path" | where_exp: "file", "file.path != cover_path" | sort: "path" %}
      {% if numbered_images.size > 0 %}
        <section class="project-additional-images">
          <h2 class="visually-hidden">Additional Images</h2>
          {% for image in numbered_images %}
            <figure class="project-image">
              <img src="{{ image.path | relative_url }}" alt="{{ page.title }} - {{ image.name }}">
            </figure>
          {% endfor %}
        </section>
      {% endif %}
    </section>
  </article>
</main>
```

**Rationale:**
- `<main>` for primary content
- `<article>` wrapper for project content (semantically correct for standalone content)
- `<header>` for project header section
- `<h1>` for project title
- `<section>` with descriptive classes for content organization
- `<figure>` for images (semantically correct for content that can stand alone)
- `<h2>` for additional images section (hidden visually but available to screen readers)

## CSS Updates Required

### _sass/_layout.scss
**Current Code (lines 22-26):**
```scss
.content {
    grid-row: 2;
    grid-column: 1 / span 6;
    padding: 0 0.5em 0.5em;
}
```

**Required Change:**
```scss
.content {
    grid-row: 2;
    grid-column: 1 / span 6;
    padding: 0 0.5em 0.5em;
}

.projects-grid {
    display: grid;
    gap: 1rem;
}

.project-preview {
    display: block;
}

.project-gallery {
    display: grid;
    gap: 1rem;
}

.project-image {
    margin: 0;
}

.visually-hidden {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}
```

## Implementation Steps

1. **Update _layouts/default.html**:
   - Replace `<section class="content">` with `<main class="content">`

2. **Update _layouts/home.html**:
   - Replace `<section class="content">` with `<main class="content">`
   - Add `<h1>{{ page.title }}</h1>`
   - Wrap posts loop in `<section class="projects-grid">`
   - Wrap each post in `<article class="project-preview">`
   - Add `<h2>{{ post.title }}</h2>` inside each post link

3. **Update _layouts/project.html**:
   - Replace `<section class="content">` with `<main class="content">`
   - Add `<article class="project">` wrapper
   - Add `<header class="project-header">` with `<h1>{{ page.title }}</h1>`
   - Replace `<div class="project-image">` with `<figure class="project-image">`
   - Add section wrapper for additional images with hidden heading

4. **Update _sass/_layout.scss**:
   - Add new CSS classes for semantic structure
   - Add visually-hidden utility class

## Testing Considerations

1. **Accessibility Testing**:
   - Use screen reader to verify heading hierarchy
   - Test keyboard navigation through semantic elements
   - Validate with accessibility tools (aXe, WAVE)

2. **Visual Testing**:
   - Ensure layout remains unchanged
   - Verify responsive behavior
   - Check all image displays correctly

3. **SEO Testing**:
   - Validate HTML structure
   - Check heading hierarchy in dev tools
   - Verify semantic elements are properly nested

## Benefits

- **Improved Accessibility**: Proper heading hierarchy and semantic elements help screen readers navigate content
- **Better SEO**: Search engines can better understand page structure and content hierarchy
- **Future-proof**: Semantic HTML is more maintainable and extensible
- **Standards Compliance**: Follows HTML5 semantic markup best practices

## Related Tasks

- This task should be completed before accessibility-enhancements.md
- May require updates to CSS structure (css-scss-structure.md)
- Complements meta-information.md for full semantic markup