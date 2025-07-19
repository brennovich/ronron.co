# Task: Content Structure

## Priority: Low

## Overview
Improve content organization, add proper heading structures, enhance post metadata, and implement better content hierarchy for improved accessibility and SEO.

## Files to Create/Modify

### 1. Update info.md
**Current Code:**
```markdown
---
layout: default
title: "Info"
---

A metaphor for the human condition, a reflection of our shared experiences, and a reminder that we are all in this together.

<img src="/assets/images/info-cat.svg" alt="Jumpy cat" width="400"/>

Jumpy cat
```

**Required Change:**
```markdown
---
layout: default
title: "Info"
description: "About ronron.co and the creative philosophy behind the projects"
---

# About ronron.co

## Creative Philosophy

A metaphor for the human condition, a reflection of our shared experiences, and a reminder that we are all in this together.

## The Creative Process

Our approach to design and craftsmanship is rooted in:

- **Simplicity**: Finding beauty in minimal, functional forms
- **Craftsmanship**: Attention to detail and quality materials
- **Storytelling**: Each piece tells a unique story
- **Sustainability**: Creating lasting, meaningful objects

## Meet the Maker

<figure class="creator-image">
  <img src="/assets/images/info-cat.svg" 
       alt="Illustration of a stylized cat in a jumping pose, representing the creative spirit of ronron.co" 
       width="400" 
       height="300">
  <figcaption>Jumpy cat - Our creative mascot</figcaption>
</figure>

## Contact

Interested in collaborating or learning more about our work? We'd love to hear from you.

- **Email**: [info@ronron.co](mailto:info@ronron.co)
- **Location**: Based in Germany
- **Availability**: Currently accepting select projects

## Project Approach

### Design Phase
We begin each project with extensive research and conceptualization, ensuring every design decision serves both form and function.

### Creation Process
Using traditional craftsmanship techniques combined with modern design principles, we create unique pieces that stand the test of time.

### Quality Assurance
Every piece undergoes rigorous quality checks to ensure it meets our high standards before reaching you.

---

*Thank you for visiting ronron.co. We hope our work inspires you as much as it inspires us to create.*
```

### 2. Update index.md
**Current Code:**
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
subtitle: "Welcome to our creative portfolio"
---

# Welcome to ronron.co

Discover a curated collection of creative projects that blend traditional craftsmanship with modern design sensibility.

## Featured Projects

Each project in our portfolio represents a unique exploration of form, function, and artistic expression.
```

### 3. Update Post Files with Enhanced Front Matter
**Example for _posts/2025-01-01-hocker.md:**
```markdown
---
title: "Hocker"
description: "A minimalist stool design that embodies the perfect balance of form and function"
date: 2025-01-01
categories: [furniture, design]
tags: [stool, minimalist, wood, craftsmanship]
featured: true
project_type: "Furniture Design"
materials: ["Oak wood", "Natural finish"]
dimensions: "45cm H × 35cm W × 35cm D"
completion_date: "January 2025"
client: "Personal Project"
cover_image: "/assets/images/posts/Hocker/cover.jpg"
cover_alt: "Minimalist wooden stool with clean lines and natural oak finish"
image_path: "/assets/images/posts/Hocker/"
gallery_title: "Hocker Design Process"
image_descriptions:
  1: "Detail view of the wood grain and craftsmanship"
  2: "Side profile showing the ergonomic design"
  3: "Close-up of the joinery techniques used"
image_captions:
  1: "Hand-selected oak wood showcasing natural grain patterns"
  2: "Carefully crafted proportions ensure comfort and stability"
  3: "Traditional joinery techniques meet modern design"
process_description: "This project began with sketching various forms and proportions, followed by material selection and prototyping."
---

# Project Overview

The Hocker represents our approach to functional minimalism. This piece demonstrates how traditional woodworking techniques can be applied to create contemporary furniture that serves both practical and aesthetic purposes.

## Design Inspiration

The inspiration for this piece came from Scandinavian design principles, emphasizing:
- Clean, uncluttered lines
- Natural material beauty
- Functional simplicity
- Timeless appeal

## Construction Details

### Materials
- **Primary**: Solid oak wood, sustainably sourced
- **Finish**: Natural oil finish to preserve wood character
- **Hardware**: None - pure wood construction

### Techniques
- Traditional mortise and tenon joinery
- Hand-sanded smooth finish
- Multiple coats of natural oil for protection

## Specifications

- **Dimensions**: 45cm H × 35cm W × 35cm D
- **Weight**: 3.2kg
- **Capacity**: Suitable for users up to 120kg
- **Maintenance**: Occasional oiling recommended

## The Making Process

This piece was crafted over several weeks, with careful attention to each step of the process. From initial sketches to final finishing, every detail was considered to ensure the final product meets our quality standards.
```

**Apply similar enhanced structure to:**
- `_posts/2025-02-01-leonard.md`
- `_posts/2025-03-01-meenhard.md`
- `_posts/2025-04-01-sideboard.md`

### 4. Create _data/projects.yml
**New file:**
```yaml
# Project metadata and organization
featured_projects:
  - title: "Hocker"
    slug: "hocker"
    date: "2025-01-01"
    featured: true
    category: "furniture"
    
  - title: "Sideboard"
    slug: "sideboard"
    date: "2025-04-01"
    featured: true
    category: "furniture"

categories:
  - name: "Furniture"
    slug: "furniture"
    description: "Functional furniture pieces designed for modern living"
    
  - name: "Design"
    slug: "design"
    description: "Conceptual design work and creative explorations"
    
  - name: "Craftsmanship"
    slug: "craftsmanship"
    description: "Showcasing traditional making techniques"

project_types:
  - name: "Furniture Design"
    description: "Custom furniture pieces for residential and commercial spaces"
    
  - name: "Product Design"
    description: "Functional objects and accessories"
    
  - name: "Conceptual Work"
    description: "Experimental and artistic projects"

materials:
  - name: "Oak Wood"
    description: "Sustainably sourced European oak"
    
  - name: "Walnut"
    description: "Premium walnut wood with rich grain"
    
  - name: "Natural Finishes"
    description: "Eco-friendly oils and waxes"

techniques:
  - name: "Traditional Joinery"
    description: "Mortise and tenon, dovetail, and other classic techniques"
    
  - name: "Hand Finishing"
    description: "Carefully hand-sanded and finished surfaces"
    
  - name: "Modern Tools"
    description: "Precision tools for contemporary accuracy"
```

### 5. Create _includes/project-metadata.liquid
**New file:**
```liquid
<!-- Project metadata display component -->
<aside class="project-metadata">
  <h2 class="metadata-title">Project Details</h2>
  
  <dl class="metadata-list">
    {% if page.project_type %}
      <dt>Type</dt>
      <dd>{{ page.project_type }}</dd>
    {% endif %}
    
    {% if page.completion_date %}
      <dt>Completed</dt>
      <dd>{{ page.completion_date }}</dd>
    {% endif %}
    
    {% if page.client %}
      <dt>Client</dt>
      <dd>{{ page.client }}</dd>
    {% endif %}
    
    {% if page.materials %}
      <dt>Materials</dt>
      <dd>
        <ul class="materials-list">
          {% for material in page.materials %}
            <li>{{ material }}</li>
          {% endfor %}
        </ul>
      </dd>
    {% endif %}
    
    {% if page.dimensions %}
      <dt>Dimensions</dt>
      <dd>{{ page.dimensions }}</dd>
    {% endif %}
    
    {% if page.tags %}
      <dt>Tags</dt>
      <dd>
        <ul class="tags-list">
          {% for tag in page.tags %}
            <li class="tag">{{ tag }}</li>
          {% endfor %}
        </ul>
      </dd>
    {% endif %}
  </dl>
</aside>
```

### 6. Create _includes/project-process.liquid
**New file:**
```liquid
<!-- Project process section -->
{% if page.process_description or page.process_steps %}
<section class="project-process">
  <h2>Design Process</h2>
  
  {% if page.process_description %}
    <p class="process-description">{{ page.process_description }}</p>
  {% endif %}
  
  {% if page.process_steps %}
    <ol class="process-steps">
      {% for step in page.process_steps %}
        <li class="process-step">
          <h3>{{ step.title }}</h3>
          <p>{{ step.description }}</p>
          {% if step.image %}
            <img src="{{ step.image | relative_url }}" alt="{{ step.title }}">
          {% endif %}
        </li>
      {% endfor %}
    </ol>
  {% endif %}
</section>
{% endif %}
```

### 7. Create _includes/table-of-contents.liquid
**New file:**
```liquid
<!-- Table of contents for long-form content -->
<nav class="table-of-contents" aria-label="Table of contents">
  <h2>Contents</h2>
  <ol class="toc-list">
    {% assign headings = content | split: '<h' %}
    {% for heading in headings %}
      {% if forloop.first %}{% continue %}{% endif %}
      
      {% assign level = heading | slice: 0, 1 | plus: 0 %}
      {% if level >= 2 and level <= 4 %}
        {% assign heading_text = heading | split: '>' | first | split: '"' | last %}
        {% assign heading_id = heading_text | slugify %}
        
        <li class="toc-item toc-level-{{ level }}">
          <a href="#{{ heading_id }}" class="toc-link">{{ heading_text }}</a>
        </li>
      {% endif %}
    {% endfor %}
  </ol>
</nav>
```

### 8. Create _includes/related-projects.liquid
**New file:**
```liquid
<!-- Related projects component -->
<section class="related-projects">
  <h2>Related Projects</h2>
  
  {% assign related_posts = site.posts | where_exp: "post", "post.url != page.url" %}
  
  {% if page.categories %}
    {% assign related_posts = related_posts | where_exp: "post", "post.categories contains page.categories.first" %}
  {% endif %}
  
  {% if related_posts.size == 0 %}
    {% assign related_posts = site.posts | where_exp: "post", "post.url != page.url" %}
  {% endif %}
  
  {% assign related_posts = related_posts | limit: 3 %}
  
  <div class="related-grid">
    {% for post in related_posts %}
      <article class="related-project">
        <a href="{{ post.url | relative_url }}" class="related-link">
          <img src="{{ post.cover_image | default: '/assets/images/posts/' | append: post.title | append: '/cover.jpg' | relative_url }}" 
               alt="{{ post.title }}"
               class="related-image">
          <h3 class="related-title">{{ post.title }}</h3>
          <p class="related-description">{{ post.description | truncate: 80 }}</p>
        </a>
      </article>
    {% endfor %}
  </div>
</section>
```

### 9. Update _layouts/project.html
**Add content structure components:**
```html
<main id="main-content" class="content" tabindex="-1">
  <article class="project">
    {% include page-header.liquid 
       title=page.title 
       subtitle=page.subtitle 
       description=page.description 
       date=page.date %}
    
    <div class="project-layout">
      <div class="project-main">
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
            {% if page.show_toc %}
              {% include table-of-contents.liquid %}
            {% endif %}
            
            {{ content }}
          </section>
        {% endif %}
        
        {% include project-process.liquid %}
      </div>
      
      <div class="project-sidebar">
        {% include project-metadata.liquid %}
      </div>
    </div>
    
    {% include related-projects.liquid %}
    
    {% include navigation-links.liquid 
       previous=page.previous 
       next=page.next 
       show_all_link=true 
       all_text='← Back to projects' %}
  </article>
</main>
```

### 10. Add CSS for Content Structure
**Add to _sass/components/_content-structure.scss:**
```scss
// Content structure styles
.project-layout {
  display: grid;
  grid-template-columns: 1fr;
  gap: $spacing-xl;
  
  @include media-min($breakpoint-lg) {
    grid-template-columns: 2fr 1fr;
  }
}

.project-main {
  min-width: 0; // Prevent grid overflow
}

.project-sidebar {
  @include media-max($breakpoint-lg) {
    order: -1;
  }
}

// Metadata styles
.project-metadata {
  background: rgba($color-primary, 0.05);
  padding: $spacing-lg;
  border-radius: $border-radius-lg;
  position: sticky;
  top: 2rem;
}

.metadata-title {
  margin: 0 0 $spacing-md 0;
  font-size: $font-size-lg;
  color: $color-primary;
}

.metadata-list {
  dt {
    font-weight: bold;
    color: $color-text;
    margin-bottom: $spacing-xs;
  }
  
  dd {
    margin: 0 0 $spacing-md 0;
    color: $color-text-light;
  }
}

.materials-list,
.tags-list {
  list-style: none;
  padding: 0;
  margin: 0;
  
  li {
    display: inline-block;
    margin-right: $spacing-sm;
    margin-bottom: $spacing-xs;
  }
}

.tag {
  background: $color-primary;
  color: $color-white;
  padding: $spacing-xs $spacing-sm;
  border-radius: $border-radius-sm;
  font-size: $font-size-xs;
  text-transform: uppercase;
  font-weight: bold;
}

// Process section
.project-process {
  margin-top: $spacing-xl;
  padding-top: $spacing-xl;
  border-top: 1px solid $color-text-light;
}

.process-steps {
  counter-reset: step-counter;
  list-style: none;
  padding: 0;
}

.process-step {
  counter-increment: step-counter;
  margin-bottom: $spacing-lg;
  padding-left: $spacing-xl;
  position: relative;
  
  &::before {
    content: counter(step-counter);
    position: absolute;
    left: 0;
    top: 0;
    width: $spacing-lg;
    height: $spacing-lg;
    background: $color-primary;
    color: $color-white;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
    font-size: $font-size-sm;
  }
}

// Table of contents
.table-of-contents {
  background: rgba($color-background, 0.8);
  padding: $spacing-md;
  border-radius: $border-radius-md;
  margin-bottom: $spacing-lg;
  border-left: 4px solid $color-primary;
}

.toc-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.toc-item {
  margin-bottom: $spacing-xs;
  
  &.toc-level-3 {
    margin-left: $spacing-md;
  }
  
  &.toc-level-4 {
    margin-left: $spacing-lg;
  }
}

.toc-link {
  text-decoration: none;
  color: $color-text;
  font-size: $font-size-sm;
  
  &:hover {
    color: $color-primary;
    text-decoration: underline;
  }
}

// Related projects
.related-projects {
  margin-top: $spacing-xxl;
  padding-top: $spacing-xl;
  border-top: 1px solid $color-text-light;
}

.related-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: $spacing-lg;
  margin-top: $spacing-lg;
}

.related-project {
  background: $color-white;
  border-radius: $border-radius-md;
  overflow: hidden;
  box-shadow: $shadow-sm;
  transition: all $transition-base;
  
  &:hover {
    box-shadow: $shadow-md;
    transform: translateY(-2px);
  }
}

.related-link {
  display: block;
  text-decoration: none;
  color: inherit;
}

.related-image {
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.related-title {
  margin: $spacing-sm 0;
  padding: 0 $spacing-md;
  font-size: $font-size-md;
  font-weight: bold;
}

.related-description {
  padding: 0 $spacing-md $spacing-md;
  margin: 0;
  font-size: $font-size-sm;
  color: $color-text-light;
}
```

## Implementation Steps

1. **Enhance Page Content**:
   - Update info.md with proper heading structure
   - Add comprehensive content sections
   - Include contact information and details

2. **Improve Post Structure**:
   - Add enhanced front matter to all posts
   - Include project metadata and descriptions
   - Add proper content structure

3. **Create Data Files**:
   - Add project organization data
   - Create category and tag structures
   - Include material and technique definitions

4. **Add Content Components**:
   - Create reusable content includes
   - Add project metadata display
   - Implement related projects section

5. **Update Templates**:
   - Enhance project layout with new structure
   - Add sidebar with metadata
   - Include table of contents for long content

## Benefits

- **Better SEO**: Proper heading structure and content organization
- **Improved Accessibility**: Clear content hierarchy for screen readers
- **Enhanced User Experience**: More informative and organized content
- **Professional Appearance**: Comprehensive project documentation
- **Better Navigation**: Related projects and table of contents

## Related Tasks

- Should be implemented after semantic-html-structure.md
- Works with meta-information.md for complete page optimization
- Complements template-organization.md improvements

## Testing Considerations

1. **Content Structure Testing**:
   - Verify heading hierarchy
   - Test table of contents functionality
   - Check related projects display

2. **SEO Testing**:
   - Validate structured data
   - Test rich snippets
   - Check content organization

3. **Accessibility Testing**:
   - Test with screen readers
   - Verify content flow
   - Check navigation landmarks

## Validation Commands

```bash
# Build and test
bundle exec jekyll build

# Check content structure
# Use heading outline tools
# Test with accessibility validators

# Validate SEO
# Use structured data testing tools
```

## Notes

- Content should be regularly updated and maintained
- Consider adding a content management workflow
- Keep project metadata consistent across all posts
- Consider adding search functionality for better content discovery