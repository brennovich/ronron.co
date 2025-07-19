# Task: Performance Optimizations

## Priority: Low

## Overview
Implement comprehensive performance optimizations including image optimization, caching strategies, asset minification, and loading performance improvements.

## Files to Create/Modify

### 1. Update _includes/head.liquid
**Add performance optimizations:**
```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  
  <!-- Preload critical resources -->
  <link rel="preload" href="{{ "/assets/css/style.css" | relative_url }}" as="style">
  <link rel="preload" href="{{ "/assets/images/logo.svg" | relative_url }}" as="image">
  
  <!-- DNS prefetch for external resources -->
  <link rel="dns-prefetch" href="//fonts.googleapis.com">
  <link rel="dns-prefetch" href="//www.google-analytics.com">
  
  <!-- Preconnect for critical third-party origins -->
  <link rel="preconnect" href="https://fonts.googleapis.com" crossorigin>
  
  <!-- Critical CSS inline (if using critical CSS) -->
  {% if site.critical_css %}
    <style>{{ site.critical_css }}</style>
  {% endif %}
  
  <!-- Non-critical CSS with media attribute -->
  <link rel="stylesheet" href="{{ "/assets/css/style.css" | relative_url }}" media="print" onload="this.media='all'">
  <noscript><link rel="stylesheet" href="{{ "/assets/css/style.css" | relative_url }}"></noscript>
  
  <!-- Title and meta tags -->
  <title>{% if page.title %}{{ page.title }} - {{ site.title }}{% else %}{{ site.title }}{% endif %}</title>
  {% include meta-tags.liquid %}
  
  <!-- Canonical URL -->
  <link rel="canonical" href="{{ page.url | absolute_url }}">
  
  <!-- Favicons with sizes -->
  <link rel="icon" type="image/x-icon" href="{{ "/assets/images/favicon.ico" | relative_url }}">
  <link rel="icon" type="image/png" sizes="16x16" href="{{ "/assets/images/favicon-16x16.png" | relative_url }}">
  <link rel="apple-touch-icon" sizes="180x180" href="{{ "/assets/images/apple-touch-icon.png" | relative_url }}">
  <link rel="icon" type="image/svg+xml" href="{{ "/assets/images/favicon.svg" | relative_url }}">
  <link rel="icon" type="image/png" sizes="512x512" href="{{ "/assets/images/android-chrome-512x512.png" | relative_url }}">
  
  <!-- Mobile optimizations -->
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black">
  <meta name="theme-color" content="#f6f4f2">
  
  <!-- Performance hints -->
  <meta name="author" content="{{ site.author | default: site.title }}">
  <meta name="generator" content="Jekyll {{ jekyll.version }}">
  
  <!-- SEO plugins -->
  {% seo %}
  {% feed_meta %}
  
  <!-- Service Worker registration -->
  {% if jekyll.environment == 'production' %}
    <script>
      if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('/sw.js');
      }
    </script>
  {% endif %}
</head>
```

### 2. Create _includes/optimized-image.liquid
**New file:**
```liquid
<!-- Optimized image component with responsive loading -->
{% assign image_path = include.src %}
{% assign image_alt = include.alt | default: include.title %}
{% assign image_width = include.width | default: 800 %}
{% assign image_height = include.height | default: 600 %}
{% assign image_loading = include.loading | default: 'lazy' %}
{% assign image_class = include.class | default: 'optimized-image' %}

<!-- Generate responsive image paths -->
{% assign image_dir = image_path | split: '/' | pop | join: '/' %}
{% assign image_name = image_path | split: '/' | last | split: '.' | first %}
{% assign image_ext = image_path | split: '.' | last %}

<!-- Picture element for modern formats -->
<picture class="{{ image_class }}">
  <!-- WebP format -->
  <source type="image/webp" 
          srcset="{{ image_dir }}/{{ image_name }}.webp {{ image_width }}w"
          sizes="{{ include.sizes | default: '(max-width: 768px) 100vw, 50vw' }}">
  
  <!-- AVIF format (if available) -->
  <source type="image/avif" 
          srcset="{{ image_dir }}/{{ image_name }}.avif {{ image_width }}w"
          sizes="{{ include.sizes | default: '(max-width: 768px) 100vw, 50vw' }}">
  
  <!-- Fallback to original format -->
  <img src="{{ image_path | relative_url }}" 
       alt="{{ image_alt }}"
       width="{{ image_width }}" 
       height="{{ image_height }}"
       loading="{{ image_loading }}"
       decoding="async"
       {% if include.priority %}fetchpriority="high"{% endif %}
       class="{{ image_class }}-img">
</picture>
```

### 3. Create assets/js/performance.js
**New file:**
```javascript
// Performance optimization utilities

// Lazy loading intersection observer
class LazyImageObserver {
  constructor() {
    this.imageObserver = new IntersectionObserver((entries, observer) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const img = entry.target;
          this.loadImage(img);
          observer.unobserve(img);
        }
      });
    }, {
      rootMargin: '50px'
    });
    
    this.init();
  }
  
  init() {
    const lazyImages = document.querySelectorAll('img[loading="lazy"]');
    lazyImages.forEach(img => {
      this.imageObserver.observe(img);
    });
  }
  
  loadImage(img) {
    img.addEventListener('load', () => {
      img.classList.add('loaded');
    });
    
    img.addEventListener('error', () => {
      img.classList.add('error');
    });
  }
}

// Critical resource preloader
class ResourcePreloader {
  constructor() {
    this.preloadedResources = new Set();
  }
  
  preload(href, as, type = null) {
    if (this.preloadedResources.has(href)) return;
    
    const link = document.createElement('link');
    link.rel = 'preload';
    link.href = href;
    link.as = as;
    if (type) link.type = type;
    
    document.head.appendChild(link);
    this.preloadedResources.add(href);
  }
  
  preloadImages(urls) {
    urls.forEach(url => {
      this.preload(url, 'image');
    });
  }
}

// Performance monitoring
class PerformanceMonitor {
  constructor() {
    this.metrics = {};
    this.init();
  }
  
  init() {
    // Monitor Core Web Vitals
    this.measureCLS();
    this.measureFID();
    this.measureLCP();
    
    // Monitor custom metrics
    this.measurePageLoadTime();
    this.measureImageLoadTime();
  }
  
  measureCLS() {
    let clsValue = 0;
    let clsEntries = [];
    
    const observer = new PerformanceObserver((list) => {
      for (const entry of list.getEntries()) {
        if (!entry.hadRecentInput) {
          clsValue += entry.value;
          clsEntries.push(entry);
        }
      }
    });
    
    observer.observe({type: 'layout-shift', buffered: true});
    this.metrics.cls = clsValue;
  }
  
  measureFID() {
    new PerformanceObserver((list) => {
      for (const entry of list.getEntries()) {
        this.metrics.fid = entry.processingStart - entry.startTime;
      }
    }).observe({type: 'first-input', buffered: true});
  }
  
  measureLCP() {
    new PerformanceObserver((list) => {
      const entries = list.getEntries();
      const lastEntry = entries[entries.length - 1];
      this.metrics.lcp = lastEntry.startTime;
    }).observe({type: 'largest-contentful-paint', buffered: true});
  }
  
  measurePageLoadTime() {
    window.addEventListener('load', () => {
      const loadTime = performance.now();
      this.metrics.pageLoadTime = loadTime;
    });
  }
  
  measureImageLoadTime() {
    const images = document.querySelectorAll('img');
    let totalImages = images.length;
    let loadedImages = 0;
    
    images.forEach(img => {
      img.addEventListener('load', () => {
        loadedImages++;
        if (loadedImages === totalImages) {
          this.metrics.allImagesLoaded = performance.now();
        }
      });
    });
  }
  
  getMetrics() {
    return this.metrics;
  }
}

// Initialize performance optimizations
document.addEventListener('DOMContentLoaded', () => {
  // Initialize lazy loading
  if ('IntersectionObserver' in window) {
    new LazyImageObserver();
  }
  
  // Initialize resource preloader
  const preloader = new ResourcePreloader();
  
  // Preload critical images on user interaction
  let hasInteracted = false;
  const preloadCriticalImages = () => {
    if (hasInteracted) return;
    hasInteracted = true;
    
    // Preload next page images
    const nextPageLinks = document.querySelectorAll('a[href*="/20"]');
    nextPageLinks.forEach(link => {
      link.addEventListener('mouseenter', () => {
        const href = link.getAttribute('href');
        if (href) {
          const imagePath = href.replace(/\/$/, '') + '/cover.jpg';
          preloader.preload(imagePath, 'image');
        }
      });
    });
  };
  
  ['mousedown', 'touchstart'].forEach(event => {
    document.addEventListener(event, preloadCriticalImages, { once: true });
  });
  
  // Initialize performance monitoring
  if (window.location.hostname !== 'localhost') {
    const monitor = new PerformanceMonitor();
    
    // Report metrics after page load
    window.addEventListener('load', () => {
      setTimeout(() => {
        const metrics = monitor.getMetrics();
        console.log('Performance metrics:', metrics);
        
        // Send to analytics if needed
        // gtag('event', 'performance_metrics', metrics);
      }, 1000);
    });
  }
});
```

### 4. Create sw.js (Service Worker)
**New file in root:**
```javascript
// Service Worker for caching
const CACHE_NAME = 'ronron-co-v1';
const urlsToCache = [
  '/',
  '/assets/css/style.css',
  '/assets/images/logo.svg',
  '/assets/images/favicon.svg',
  '/assets/js/performance.js'
];

// Install event
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => cache.addAll(urlsToCache))
  );
});

// Fetch event
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request)
      .then(response => {
        // Return cached version or fetch from network
        return response || fetch(event.request);
      })
  );
});

// Activate event
self.addEventListener('activate', event => {
  event.waitUntil(
    caches.keys().then(cacheNames => {
      return Promise.all(
        cacheNames.map(cacheName => {
          if (cacheName !== CACHE_NAME) {
            return caches.delete(cacheName);
          }
        })
      );
    })
  );
});
```

### 5. Update _config.yml
**Add performance-related configurations:**
```yaml
# Performance settings
compress_html:
  clippings: all
  comments: ["<!-- ", " -->"]
  endings: all
  profile: false
  blanklines: false
  ignore:
    envs: [development]

# Sass compression
sass:
  style: compressed

# Plugin settings
plugins:
  - jekyll-compress-html
  - jekyll-responsive-image
  - jekyll-minifier

# Responsive images
responsive_image:
  template: _includes/responsive-image.html
  sizes:
    - width: 400
      quality: 80
    - width: 800
      quality: 80
    - width: 1200
      quality: 80
  strip: true
  save_to_source: false
  output_path_format: assets/images/resized/%{dirname}/%{basename}-%{width}.%{extension}

# Minification settings
jekyll-minifier:
  uglifier_args:
    harmony: true
  compress_css: true
  compress_javascript: true
  compress_json: true
  simple_doctype: true
  remove_spaces_inside_tags: true
  remove_multi_spaces: true
  remove_comments: true
  remove_intertag_spaces: true
  remove_quotes: false
  compress_css_files:
    - /assets/css/style.css
  compress_js_files:
    - /assets/js/performance.js
```

### 6. Create _includes/critical-css.liquid
**New file:**
```liquid
<!-- Critical CSS for above-the-fold content -->
<style>
  /* Critical styles for header and initial content */
  body {
    font-family: "Courier New", monospace;
    background-color: #f6f4f2;
    margin: 0;
    display: grid;
    grid-template-rows: auto 1fr;
    min-height: 100vh;
  }
  
  header {
    position: sticky;
    top: 0.5em;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.5em 1em;
    background-color: rgba(246, 244, 242, 0.9);
    backdrop-filter: blur(10px);
    z-index: 1020;
  }
  
  .logo {
    width: 5em;
    height: 1.2em;
  }
  
  .logo img {
    width: 100%;
    height: auto;
    display: block;
  }
  
  .nav a {
    color: #6000ea;
    text-decoration: none;
    font-weight: bold;
    text-transform: uppercase;
    font-size: 1.8em;
  }
  
  .content {
    padding: 0 1em 1em;
  }
  
  .skip-link {
    position: absolute;
    top: -40px;
    left: 6px;
    background: #6000ea;
    color: white;
    padding: 8px;
    text-decoration: none;
    z-index: 1070;
    border-radius: 4px;
    transition: top 0.3s;
  }
  
  .skip-link:focus {
    top: 6px;
  }
  
  /* Loading states */
  img {
    opacity: 0;
    transition: opacity 0.3s;
  }
  
  img.loaded {
    opacity: 1;
  }
  
  img.error {
    opacity: 0.5;
  }
</style>
```

### 7. Update assets/css/style.scss
**Add performance-related styles:**
```scss
---
---

@import "main";

// Performance optimizations
img {
  opacity: 0;
  transition: opacity 0.3s ease;
  
  &.loaded {
    opacity: 1;
  }
  
  &.error {
    opacity: 0.5;
  }
}

// Optimize for will-change
.project-card {
  will-change: transform;
}

.project-card:hover {
  will-change: auto;
}

// Contain layout for better performance
.projects-grid {
  contain: layout;
}

.project-image {
  contain: layout;
}

// Optimize animations
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

// Print styles
@media print {
  .nav,
  .skip-link,
  .project-navigation {
    display: none;
  }
  
  .content {
    padding: 0;
  }
  
  img {
    max-width: 100% !important;
    height: auto !important;
  }
}
```

### 8. Create .htaccess (for Apache servers)
**New file:**
```apache
# Performance optimizations

# Enable compression
<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/plain
    AddOutputFilterByType DEFLATE text/html
    AddOutputFilterByType DEFLATE text/xml
    AddOutputFilterByType DEFLATE text/css
    AddOutputFilterByType DEFLATE application/xml
    AddOutputFilterByType DEFLATE application/xhtml+xml
    AddOutputFilterByType DEFLATE application/rss+xml
    AddOutputFilterByType DEFLATE application/javascript
    AddOutputFilterByType DEFLATE application/x-javascript
</IfModule>

# Set expire headers
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType image/jpg "access plus 1 month"
    ExpiresByType image/jpeg "access plus 1 month"
    ExpiresByType image/gif "access plus 1 month"
    ExpiresByType image/png "access plus 1 month"
    ExpiresByType image/svg+xml "access plus 1 month"
    ExpiresByType text/css "access plus 1 month"
    ExpiresByType application/pdf "access plus 1 month"
    ExpiresByType text/javascript "access plus 1 month"
    ExpiresByType application/javascript "access plus 1 month"
    ExpiresByType application/x-javascript "access plus 1 month"
    ExpiresByType application/x-shockwave-flash "access plus 1 month"
    ExpiresByType image/x-icon "access plus 1 year"
    ExpiresDefault "access plus 2 days"
</IfModule>

# Cache control
<IfModule mod_headers.c>
    <FilesMatch "\.(css|js|png|jpg|jpeg|gif|svg|woff|woff2)$">
        Header set Cache-Control "max-age=31536000, public"
    </FilesMatch>
</IfModule>
```

## Implementation Steps

1. **Image Optimization**:
   - Implement responsive images with srcset
   - Add WebP and AVIF format support
   - Implement lazy loading with intersection observer

2. **Asset Optimization**:
   - Minify CSS and JavaScript
   - Implement critical CSS inlining
   - Add resource preloading

3. **Caching Strategy**:
   - Implement service worker caching
   - Add proper cache headers
   - Optimize asset delivery

4. **Performance Monitoring**:
   - Add Core Web Vitals monitoring
   - Implement custom performance metrics
   - Add performance budgets

5. **Build Optimization**:
   - Configure Jekyll for production
   - Add compression settings
   - Implement asset pipeline

## Testing Considerations

1. **Performance Testing**:
   - Test with Lighthouse
   - Measure Core Web Vitals
   - Test on slow connections
   - Monitor build times

2. **Image Testing**:
   - Test responsive images
   - Verify lazy loading
   - Check WebP/AVIF support
   - Test fallback behavior

3. **Caching Testing**:
   - Test service worker
   - Verify cache headers
   - Test offline functionality
   - Check cache invalidation

4. **Cross-browser Testing**:
   - Test modern features
   - Verify fallbacks
   - Check performance across browsers

## Performance Targets

- **First Contentful Paint**: < 1.5s
- **Largest Contentful Paint**: < 2.5s
- **First Input Delay**: < 100ms
- **Cumulative Layout Shift**: < 0.1
- **Lighthouse Performance Score**: > 90

## Benefits

- **Improved User Experience**: Faster loading times and smoother interactions
- **Better SEO**: Performance is a ranking factor
- **Reduced Bounce Rate**: Faster sites keep users engaged
- **Lower Hosting Costs**: Efficient resource usage
- **Mobile Performance**: Optimized for mobile devices

## Related Tasks

- Should be implemented after all other tasks
- Complements image-accessibility.md optimizations
- Works with jekyll-configuration.md settings

## Validation Commands

```bash
# Build for production
JEKYLL_ENV=production bundle exec jekyll build

# Test performance
# Use Lighthouse CLI
npx lighthouse https://ronron.co --output=json --output-path=./lighthouse-report.json

# Test image optimization
# Use ImageOptim or similar tools

# Test service worker
# Use Chrome DevTools Application tab
```

## Notes

- Performance optimizations should be tested thoroughly
- Some optimizations may not be compatible with GitHub Pages
- Consider using a CDN for better global performance
- Monitor performance metrics regularly
- Keep performance budgets updated