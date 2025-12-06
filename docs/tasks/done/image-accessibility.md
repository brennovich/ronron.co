# Task: Enhance Image Accessibility

## Priority: High

## Overview
Improve alt text quality, add proper image attributes, implement responsive images, and enhance image performance for better accessibility and user experience.

## Files to Modify

### 1. _layouts/home.html
**Current Code (line 10):**
```html
<img src="{{ '/assets/images/posts/' | append: post.title | append: '/cover.jpg' | relative_url }}" alt="{{ post.title }}">
```

**Required Change:**
```html
<img src="{{ '/assets/images/posts/' | append: post.title | append: '/cover.jpg' | relative_url }}" 
     alt="{{ post.description | default: post.title | append: ' project cover image' }}"
     loading="lazy">
```

**Rationale:** 
- More descriptive alt text using description if available
- Width/height attributes prevent layout shift
- Lazy loading improves performance for images below the fold

### 2. _layouts/project.html
**Current Code (lines 8-11):**
```html
{% assign cover_path = '/assets/images/posts/' | append: page.title | append: '/cover.jpg' %}
<div class="project-image">
  <img src="{{ cover_path | relative_url }}" alt="{{ page.title }} - Cover">
</div>
```

**Required Change:**
```html
{% assign cover_path = '/assets/images/posts/' | append: page.title | append: '/cover.jpg' %}
<figure class="project-image project-cover">
  <img src="{{ cover_path | relative_url }}" 
       alt="{{ page.description | default: page.title | append: ' - Main project image showing the complete work' }}">
  {% if page.image_caption %}
    <figcaption>{{ page.image_caption }}</figcaption>
  {% endif %}
</figure>
```

**Current Code (lines 15-18):**
```html
{% for image in numbered_images %}
  <div class="project-image">
    <img src="{{ image.path | relative_url }}" alt="{{ page.title }} - {{ image.name }}">
  </div>
{% endfor %}
```

**Required Change:**
```html
{% for image in numbered_images %}
  {% assign image_number = forloop.index %}
  {% assign image_description = page.image_descriptions[image_number] | default: 'Detail view' %}
  <figure class="project-image">
    <img src="{{ image.path | relative_url }}" 
         alt="{{ page.title }} - {{ image_description }} (Image {{ image_number }} of {{ numbered_images.size }})"
         loading="lazy">
    {% if page.image_captions[image_number] %}
      <figcaption>{{ page.image_captions[image_number] }}</figcaption>
    {% endif %}
  </figure>
{% endfor %}
```

### 3. info.md
**Current Code (line 8):**
```html
<img src="/assets/images/info-cat.svg" alt="Jumpy cat" width="400"/>
```

**Required Change:**
```html
<img src="/assets/images/info-cat.svg" 
     alt="Illustration of a stylized cat in a jumping or playful pose, representing the creative spirit of ronron.co" 
     width="400" 
     height="300"
     loading="lazy">
```

### 4. Update Post Front Matter
**Add to each post in _posts/ (example for 2025-01-01-hocker.md):**
```markdown
---
title: Hocker
description: "Hocker project - A creative exploration in design and functionality"
image_caption: "The completed Hocker design showcasing clean lines and functional aesthetics"
image_descriptions:
  1: "Detail view of the construction and materials"
  2: "Close-up of the joinery and craftsmanship"
  3: "Alternative angle showing the form and proportions"
image_captions:
  1: "Construction detail highlighting the precision woodworking"
  2: "Hand-crafted joinery showcasing traditional techniques"
  3: "Side profile emphasizing the ergonomic design"
---
```

**Apply similar structure to:**
- `_posts/2025-02-01-leonard.md`
- `_posts/2025-03-01-meenhard.md`
- `_posts/2025-04-01-sideboard.md`

### 5. Create Image Optimization Include
**Create new file: `_includes/responsive-image.liquid`**
```liquid
<!-- Responsive image include -->
{% assign image_path = include.src %}
{% assign image_alt = include.alt | default: include.title %}
{% assign image_width = include.width | default: 800 %}
{% assign image_loading = include.loading | default: 'lazy' %}
{% assign image_class = include.class | default: 'responsive-image' %}

<img src="{{ image_path | relative_url }}" 
     alt="{{ image_alt }}"
     width="{{ image_width }}" 
     loading="{{ image_loading }}"
     class="{{ image_class }}">
```

## CSS Updates Required

### _sass/_layout.scss
**Add to existing file:**
```scss
// Image loading states
img[loading="lazy"] {
    opacity: 0;
    transition: opacity 0.3s;
}

img[loading="lazy"].loaded {
    opacity: 1;
}
```

## Implementation Steps

1. **Update Layout Templates**:
   - Modify `_layouts/home.html` with improved alt text and attributes
   - Update `_layouts/project.html` with figure elements and enhanced descriptions
   - Fix `info.md` image attributes

2. **Create Responsive Image Include**:
   - Add `_includes/responsive-image.liquid` for reusable image component
   - Update layouts to use the include where appropriate

3. **Update Post Front Matter**:
   - Add descriptive metadata to all posts
   - Include image descriptions and captions
   - Add proper descriptions for alt text generation

4. **Add CSS Enhancements**:
   - Include loading animation styles

## Testing Considerations

1. **Accessibility Testing**:
   - Test with screen readers (NVDA, JAWS, VoiceOver)
   - Verify alt text is descriptive and contextual
   - Check keyboard navigation through images
   - Validate with aXe or WAVE accessibility tools

2. **Performance Testing**:
   - Measure layout shift (CLS) improvements
   - Test lazy loading behavior
   - Verify image optimization benefits
   - Check loading times with slow connections

3. **Visual Testing**:
   - Test responsive behavior across devices
   - Verify image quality and aspect ratios
   - Check caption positioning and styling
   - Test with images disabled

4. **Browser Compatibility**:
   - Test lazy loading support
   - Verify figure/figcaption display
   - Check responsive image behavior

## Benefits

- **Enhanced Accessibility**: Descriptive alt text helps screen reader users understand image content
- **Better Performance**: Lazy loading and optimized attributes reduce initial page load
- **Improved SEO**: Proper image attributes help search engines understand content
- **Professional Appearance**: Captions and proper image structure enhance visual presentation
- **Future-proof**: Extensible structure for additional image formats (WebP, AVIF)

## Related Tasks

- Should be implemented after semantic-html-structure.md
- Complements accessibility-enhancements.md
- Works with performance-optimizations.md for complete image optimization

## Validation Commands

```bash
# Build site locally
bundle exec jekyll serve

# Test accessibility
# Use browser dev tools to inspect image attributes
# Test with screen reader
# Validate HTML structure
```
