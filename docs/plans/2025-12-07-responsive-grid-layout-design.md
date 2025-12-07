# Responsive Grid Layout Design

Date: 2025-12-07

## Overview

Implement a progressive multi-column grid layout for the projects gallery that scales from 1 column on mobile to 4 columns on large desktop screens, using a hybrid approach combining CSS Grid auto-fit with breakpoint constraints.

## Requirements

- Progressive column increase: 1 → 2 → 3 → 4 columns
- Breakpoints: 768px, 1024px, 1440px
- Progressive gap increase: 1rem → 1.5rem → 2rem
- Prevent awkward partial columns at in-between viewport sizes

## Strategy

### Hybrid CSS Grid Approach

Combines CSS Grid's `auto-fit` flexibility with container max-width constraints:

- `auto-fit` with `minmax()` provides automatic column flow based on available space
- Container max-widths at breakpoints ensure target column counts
- Prevents awkward layouts at odd viewport sizes

### Grid Base

```scss
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 300px), 1fr));
  gap: 1rem;
}
```

Minimum item width: 300px (adjustable based on content needs)

### Gap Progression

```scss
.projects-grid {
  gap: 1rem;  // mobile baseline

  @media (min-width: 768px) {
    gap: 1.5rem;
  }

  @media (min-width: 1440px) {
    gap: 2rem;
  }
}
```

### Column Control via Container Width

```scss
.content {
  max-width: none;  // mobile: full width

  @media (min-width: 768px) {
    max-width: 800px;  // ~2 columns with gaps
    margin: 0 auto;
  }

  @media (min-width: 1024px) {
    max-width: 1200px;  // 3 columns
  }

  @media (min-width: 1440px) {
    max-width: 1600px;  // 4 columns
  }
}
```

Max-widths will be tuned based on actual item size (280-320px per column).

## Breakpoint Behavior

| Viewport Width | Columns | Gap    | Container Max-Width |
|----------------|---------|--------|---------------------|
| < 768px        | 1       | 1rem   | none                |
| 768px-1023px   | 2       | 1.5rem | 800px               |
| 1024px-1439px  | 3       | 1.5rem | 1200px              |
| 1440px+        | 4       | 2rem   | 1600px              |

## Edge Cases

### Very Wide Screens (2000px+)
4-column layout with generous gaps remains visually balanced. Optional: hard cap at `max-width: 1920px` if needed.

### In-Between Viewport Sizes
`auto-fit` with `minmax()` handles awkward widths gracefully - extra space goes to gaps/margins rather than creating partial columns.

### Aspect Ratio Handling
Existing responsive image system (`<picture>` element) already handles this. No additional work needed.

## Testing Checklist

- Mobile (375px, 414px): single column, 1rem gap
- Tablet (768px, 820px): 2 columns, 1.5rem gap
- Small desktop (1024px, 1280px): 3 columns, 1.5rem gap
- Large desktop (1440px+): 4 columns, 2rem gap
- Odd sizes (900px, 1100px): smooth transitions without partial columns

## Browser Support

CSS Grid with `auto-fit` and `minmax()` supported in all modern browsers. Compatible with existing CSS usage in project.

## Implementation Files

- `_sass/_layout.scss`: Grid and container modifications
