# Task: Improve CSS/SCSS Structure

## Priority: Medium

## Overview
Enhance SCSS organization, add useful mixins, create responsive utilities, and improve maintainability of the stylesheet architecture.

## Files to Modify

### 1. _sass/_theme.scss
**Current Code (lines 1-14):**
```scss
$background-color: #f6f4f2;
$theme-color: #6000ea;

@mixin hover-text {
  box-shadow: inset 0 -2px 0px $theme-color;
}

@mixin active-text {
  box-shadow: inset 0 -3px 0px $theme-color;
}

body {
  background-color: $background-color;
}
```

**Required Change:**
```scss
// Color palette
$color-background: #f6f4f2;
$color-primary: #6000ea;
$color-text: #333;
$color-text-light: #666;
$color-text-muted: #999;
$color-white: #fff;
$color-black: #000;

// Spacing scale
$spacing-xs: 0.25rem;
$spacing-sm: 0.5rem;
$spacing-md: 1rem;
$spacing-lg: 1.5rem;
$spacing-xl: 2rem;
$spacing-xxl: 3rem;

// Typography scale
$font-size-xs: 0.75rem;
$font-size-sm: 0.875rem;
$font-size-base: 1rem;
$font-size-lg: 1.125rem;
$font-size-xl: 1.25rem;
$font-size-xxl: 1.5rem;
$font-size-xxxl: 1.875rem;

// Line heights
$line-height-tight: 1.2;
$line-height-normal: 1.5;
$line-height-loose: 1.8;

// Breakpoints
$breakpoint-sm: 576px;
$breakpoint-md: 768px;
$breakpoint-lg: 992px;
$breakpoint-xl: 1200px;

// Z-index scale
$z-index-dropdown: 1000;
$z-index-sticky: 1020;
$z-index-fixed: 1030;
$z-index-modal: 1050;
$z-index-tooltip: 1070;

// Transitions
$transition-base: 0.3s ease;
$transition-fast: 0.15s ease;
$transition-slow: 0.5s ease;

// Shadows
$shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.1);
$shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
$shadow-lg: 0 10px 20px rgba(0, 0, 0, 0.1);

// Border radius
$border-radius-sm: 0.25rem;
$border-radius-md: 0.5rem;
$border-radius-lg: 1rem;

// Mixins
@mixin hover-text {
  box-shadow: inset 0 -2px 0px $color-primary;
  transition: box-shadow $transition-fast;
}

@mixin active-text {
  box-shadow: inset 0 -3px 0px $color-primary;
}

@mixin focus-visible {
  outline: 2px solid $color-primary;
  outline-offset: 2px;
}

@mixin sr-only {
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

// Responsive mixins
@mixin media-min($breakpoint) {
  @media (min-width: $breakpoint) {
    @content;
  }
}

@mixin media-max($breakpoint) {
  @media (max-width: $breakpoint - 1px) {
    @content;
  }
}

@mixin media-between($min, $max) {
  @media (min-width: $min) and (max-width: $max - 1px) {
    @content;
  }
}

// Animation mixins
@mixin fade-in($duration: $transition-base) {
  opacity: 0;
  animation: fade-in $duration ease-out forwards;
}

@keyframes fade-in {
  to { opacity: 1; }
}

// Base styles
body {
  background-color: $color-background;
  color: $color-text;
  transition: background-color $transition-base;
}
```

### 2. _sass/_layout.scss
**Current Code (lines 1-26):**
```scss
body {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    grid-template-rows: repeat(2, minmax(2em, auto));
}

header {
    position: sticky;
    display: flex;
    grid-column: 1 / span 6;
    top: .5em;

    .logo {
        flex: 1;
    }

    .nav {
        flex: 1;
    }
}

.content {
    grid-row: 2;
    grid-column: 1 / span 6;
    padding: 0 0.5em 0.5em;
}
```

**Required Change:**
```scss
// Base layout
body {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    grid-template-rows: auto 1fr;
    min-height: 100vh;
    gap: $spacing-md;
    
    @include media-min($breakpoint-lg) {
        grid-template-columns: repeat(12, 1fr);
        max-width: 1200px;
        margin: 0 auto;
        padding: 0 $spacing-md;
    }
}

// Header layout
header {
    position: sticky;
    display: flex;
    grid-column: 1 / -1;
    top: $spacing-sm;
    align-items: center;
    justify-content: space-between;
    padding: $spacing-sm $spacing-md;
    background-color: rgba($color-background, 0.9);
    backdrop-filter: blur(10px);
    z-index: $z-index-sticky;
    
    @include media-min($breakpoint-md) {
        padding: $spacing-md $spacing-lg;
    }
}

// Main content
.content {
    grid-row: 2;
    grid-column: 1 / -1;
    padding: 0 $spacing-md $spacing-lg;
    
    @include media-min($breakpoint-md) {
        padding: 0 $spacing-lg $spacing-xl;
    }
}

// Grid utilities
.grid {
    display: grid;
    gap: $spacing-md;
    
    &--2-cols {
        grid-template-columns: repeat(2, 1fr);
    }
    
    &--3-cols {
        grid-template-columns: repeat(3, 1fr);
        
        @include media-max($breakpoint-md) {
            grid-template-columns: 1fr;
        }
    }
    
    &--auto-fit {
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    }
}

// Flexbox utilities
.flex {
    display: flex;
    
    &--center {
        align-items: center;
        justify-content: center;
    }
    
    &--between {
        align-items: center;
        justify-content: space-between;
    }
    
    &--column {
        flex-direction: column;
    }
    
    &--wrap {
        flex-wrap: wrap;
    }
}

// Container utilities
.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 $spacing-md;
    
    &--narrow {
        max-width: 800px;
    }
    
    &--wide {
        max-width: 1400px;
    }
}

// Spacing utilities
.mt-auto { margin-top: auto; }
.mb-auto { margin-bottom: auto; }
.ml-auto { margin-left: auto; }
.mr-auto { margin-right: auto; }

@each $size, $value in (xs: $spacing-xs, sm: $spacing-sm, md: $spacing-md, lg: $spacing-lg, xl: $spacing-xl, xxl: $spacing-xxl) {
    .mt-#{$size} { margin-top: $value; }
    .mb-#{$size} { margin-bottom: $value; }
    .ml-#{$size} { margin-left: $value; }
    .mr-#{$size} { margin-right: $value; }
    .mx-#{$size} { margin-left: $value; margin-right: $value; }
    .my-#{$size} { margin-top: $value; margin-bottom: $value; }
    .m-#{$size} { margin: $value; }
    
    .pt-#{$size} { padding-top: $value; }
    .pb-#{$size} { padding-bottom: $value; }
    .pl-#{$size} { padding-left: $value; }
    .pr-#{$size} { padding-right: $value; }
    .px-#{$size} { padding-left: $value; padding-right: $value; }
    .py-#{$size} { padding-top: $value; padding-bottom: $value; }
    .p-#{$size} { padding: $value; }
}
```

### 3. Create _sass/_utilities.scss
**New file:**
```scss
// Utility classes for common patterns

// Display utilities
.hidden { display: none; }
.block { display: block; }
.inline { display: inline; }
.inline-block { display: inline-block; }
.flex { display: flex; }
.grid { display: grid; }

// Visibility utilities
.invisible { visibility: hidden; }
.visible { visibility: visible; }

// Position utilities
.relative { position: relative; }
.absolute { position: absolute; }
.fixed { position: fixed; }
.sticky { position: sticky; }

// Text utilities
.text-left { text-align: left; }
.text-center { text-align: center; }
.text-right { text-align: right; }
.text-justify { text-align: justify; }

.text-uppercase { text-transform: uppercase; }
.text-lowercase { text-transform: lowercase; }
.text-capitalize { text-transform: capitalize; }

.font-bold { font-weight: bold; }
.font-normal { font-weight: normal; }
.font-light { font-weight: 300; }

.italic { font-style: italic; }
.not-italic { font-style: normal; }

// Color utilities
.text-primary { color: $color-primary; }
.text-muted { color: $color-text-muted; }
.text-light { color: $color-text-light; }

// Background utilities
.bg-primary { background-color: $color-primary; }
.bg-white { background-color: $color-white; }
.bg-transparent { background-color: transparent; }

// Border utilities
.border { border: 1px solid $color-text-light; }
.border-top { border-top: 1px solid $color-text-light; }
.border-bottom { border-bottom: 1px solid $color-text-light; }
.border-left { border-left: 1px solid $color-text-light; }
.border-right { border-right: 1px solid $color-text-light; }

.rounded { border-radius: $border-radius-md; }
.rounded-sm { border-radius: $border-radius-sm; }
.rounded-lg { border-radius: $border-radius-lg; }
.rounded-full { border-radius: 50%; }

// Shadow utilities
.shadow-sm { box-shadow: $shadow-sm; }
.shadow-md { box-shadow: $shadow-md; }
.shadow-lg { box-shadow: $shadow-lg; }
.shadow-none { box-shadow: none; }

// Transition utilities
.transition { transition: all $transition-base; }
.transition-fast { transition: all $transition-fast; }
.transition-slow { transition: all $transition-slow; }

// Overflow utilities
.overflow-hidden { overflow: hidden; }
.overflow-visible { overflow: visible; }
.overflow-auto { overflow: auto; }

// Cursor utilities
.cursor-pointer { cursor: pointer; }
.cursor-default { cursor: default; }
.cursor-not-allowed { cursor: not-allowed; }

// Responsive utilities
@include media-max($breakpoint-sm) {
    .hidden-sm { display: none; }
    .block-sm { display: block; }
}

@include media-max($breakpoint-md) {
    .hidden-md { display: none; }
    .block-md { display: block; }
}

@include media-max($breakpoint-lg) {
    .hidden-lg { display: none; }
    .block-lg { display: block; }
}

// Accessibility utilities
.sr-only {
    @include sr-only;
}

.focus-visible:focus-visible {
    @include focus-visible;
}

// Animation utilities
.fade-in {
    @include fade-in;
}

.hover-lift {
    transition: transform $transition-fast;
    
    &:hover {
        transform: translateY(-2px);
    }
}

.hover-scale {
    transition: transform $transition-fast;
    
    &:hover {
        transform: scale(1.05);
    }
}
```

### 4. Update _sass/components/_nav.scss
**Current Code (lines 1-19):**
```scss
.nav {
    text-align: right;
    text-transform: uppercase;
    font-weight: bold;
}

.nav a {
    text-decoration: none;
    color: $theme-color;
    font-size: 1.8em;
}

.nav a:hover {
    @include hover-text;
}

.nav a:active {
    @include active-text;
}
```

**Required Change:**
```scss
.nav {
    text-align: right;
    text-transform: uppercase;
    font-weight: bold;
    
    a {
        text-decoration: none;
        color: $color-primary;
        font-size: $font-size-xl;
        padding: $spacing-sm $spacing-md;
        border-radius: $border-radius-sm;
        transition: all $transition-fast;
        
        &:hover {
            @include hover-text;
        }
        
        &:active {
            @include active-text;
        }
        
        &:focus-visible {
            @include focus-visible;
        }
        
        @include media-max($breakpoint-sm) {
            font-size: $font-size-lg;
            padding: $spacing-xs $spacing-sm;
        }
    }
}
```

### 5. Update _sass/components/_logo.scss
**Current Code (lines 1-20):**
```scss
.logo {
  width: 5em;
  height: 1.2em;
  cursor: pointer;
  padding-top: .13em;

  &:hover {
    @include hover-text;
  }

  &:active {
    @include active-text;
  }
}

.logo a {
    display: block;
    cursor: pointer;
}
```

**Required Change:**
```scss
.logo {
    width: 5em;
    height: 1.2em;
    padding-top: 0.13em;
    
    a {
        display: block;
        cursor: pointer;
        padding: $spacing-xs;
        border-radius: $border-radius-sm;
        transition: all $transition-fast;
        
        &:hover {
            @include hover-text;
        }
        
        &:active {
            @include active-text;
        }
        
        &:focus-visible {
            @include focus-visible;
        }
        
        img {
            width: 100%;
            height: auto;
            display: block;
        }
    }
    
    @include media-max($breakpoint-sm) {
        width: 4em;
        height: 1em;
    }
}
```

### 6. Update _sass/main.scss
**Current Code (lines 1-8):**
```scss
@import
    'reset',
    'theme',
    'components/logo',
    'components/nav',
    'layout',
    'typography',
    'header';
```

**Required Change:**
```scss
@import
    'theme',
    'reset',
    'utilities',
    'typography',
    'layout',
    'header',
    'components/logo',
    'components/nav';
```

## Implementation Steps

1. **Update Theme Variables**:
   - Expand `_theme.scss` with comprehensive variable system
   - Add responsive mixins and utility functions
   - Create consistent spacing and typography scales

2. **Enhance Layout System**:
   - Improve `_layout.scss` with responsive grid system
   - Add flexbox and grid utilities
   - Create container and spacing utilities

3. **Create Utility Classes**:
   - Add `_utilities.scss` with common utility classes
   - Include responsive visibility utilities
   - Add accessibility and animation utilities

4. **Update Components**:
   - Enhance navigation with better responsive behavior
   - Improve logo component with focus states
   - Add proper hover and focus interactions

5. **Reorganize Import Order**:
   - Update `main.scss` import order for better cascade
   - Ensure utilities are available to components

## Testing Considerations

1. **Responsive Testing**:
   - Test across all breakpoints
   - Verify utility classes work correctly
   - Check grid and flexbox behavior

2. **Accessibility Testing**:
   - Test focus indicators
   - Verify screen reader utility classes
   - Check keyboard navigation

3. **Performance Testing**:
   - Measure CSS file size impact
   - Test compilation speed
   - Verify unused CSS removal

## Benefits

- **Better Maintainability**: Consistent variable system and utility classes
- **Improved Responsiveness**: Comprehensive breakpoint system
- **Enhanced Accessibility**: Focus states and screen reader utilities
- **Performance**: Efficient utility-based approach
- **Scalability**: Organized architecture for future additions

## Related Tasks

- Should be implemented with semantic-html-structure.md
- Supports accessibility-enhancements.md goals
- Enhances performance-optimizations.md benefits

## Validation Commands

```bash
# Compile and check for errors
bundle exec jekyll build

# Check file sizes
ls -la _site/assets/css/

# Test responsive behavior
# Use browser dev tools to test breakpoints
```

## Notes

- Consider using CSS custom properties for better browser support
- Utility classes should be used sparingly to avoid bloat
- Test thoroughly across different devices and browsers
- Consider adding CSS linting for consistency