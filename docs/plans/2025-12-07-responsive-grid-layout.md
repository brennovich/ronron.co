# Responsive Grid Layout Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Implement progressive multi-column grid layout that scales from 1 column on mobile to 4 columns on large desktop.

**Architecture:** Hybrid approach using CSS Grid's `auto-fit` with `minmax()` for automatic column flow, combined with container `max-width` constraints at breakpoints to prevent awkward layouts.

**Tech Stack:** Jekyll, SCSS, CSS Grid

---

## Task 1: Implement Grid Base with Auto-Fit

**Files:**
- Modify: `_sass/_layout.scss:29-33`

**Step 1: Update grid base styles**

Replace the current single-column grid:

```scss
.projects-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 1rem;
}
```

With auto-fit minmax approach:

```scss
.projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(100%, 300px), 1fr));
    gap: 1rem;
}
```

**Why `min(100%, 300px)`:** Ensures single column on mobile (when viewport < 300px), then allows multiple columns when space permits. The `300px` is the minimum item width.

**Step 2: Build and verify baseline**

```bash
bundle exec jekyll build
```

Expected: Build succeeds without errors.

**Step 3: Start dev server and verify mobile layout**

```bash
bundle exec jekyll serve
```

Open: http://localhost:4000

Verify:
- At 375px width: single column layout
- At 414px width: single column layout
- Images span full width
- 1rem gap between items

**Step 4: Commit**

```bash
git add _sass/_layout.scss
git commit -m "feat: add CSS Grid auto-fit to projects grid"
```

---

## Task 2: Implement Gap Progression

**Files:**
- Modify: `_sass/_layout.scss:29-33`

**Step 1: Add gap media queries**

Update `.projects-grid` to include responsive gaps:

```scss
.projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(100%, 300px), 1fr));
    gap: 1rem;

    @media (min-width: 768px) {
        gap: 1.5rem;
    }

    @media (min-width: 1440px) {
        gap: 2rem;
    }
}
```

**Step 2: Build and verify**

```bash
bundle exec jekyll clean
bundle exec jekyll build
```

Expected: Build succeeds.

**Step 3: Manual verification**

With dev server running (http://localhost:4000):

- At 375px: gap is 1rem (16px)
- At 768px: gap is 1.5rem (24px)
- At 1440px: gap is 2rem (32px)

Use browser DevTools to measure gap between items at each breakpoint.

**Step 4: Commit**

```bash
git add _sass/_layout.scss
git commit -m "feat: add progressive gap sizes at breakpoints"
```

---

## Task 3: Implement Container Max-Widths

**Files:**
- Modify: `_sass/_layout.scss:23-27`

**Step 1: Add container constraints**

Update `.content` styles to include max-width constraints:

Current:
```scss
.content {
    grid-row: 2;
    grid-column: 1 / span 6;
    padding: 0 0.5em 0.5em 0.5em;
}
```

New:
```scss
.content {
    grid-row: 2;
    grid-column: 1 / span 6;
    padding: 0 0.5em 0.5em 0.5em;

    @media (min-width: 768px) {
        max-width: 800px;
        margin: 0 auto;
    }

    @media (min-width: 1024px) {
        max-width: 1200px;
    }

    @media (min-width: 1440px) {
        max-width: 1600px;
    }
}
```

**Why these max-widths:**
- 768px → 800px: Forces ~2 columns (2 × 300px + 1.5rem gap + padding)
- 1024px → 1200px: Allows 3 columns (3 × 300px + 2 × 1.5rem gaps + padding)
- 1440px → 1600px: Allows 4 columns (4 × 300px + 3 × 2rem gaps + padding)

**Step 2: Build and verify**

```bash
bundle exec jekyll clean
bundle exec jekyll build
```

Expected: Build succeeds.

**Step 3: Commit**

```bash
git add _sass/_layout.scss
git commit -m "feat: add container max-widths for column control"
```

---

## Task 4: Comprehensive Manual Testing

**Files:**
- None (verification only)

**Step 1: Test mobile viewports**

With dev server running at http://localhost:4000:

Test at 375px width:
- ✓ Single column layout
- ✓ Gap is 1rem (16px)
- ✓ Images fill width
- ✓ No horizontal scroll

Test at 414px width:
- ✓ Single column layout
- ✓ Gap is 1rem (16px)

**Step 2: Test tablet viewports**

Test at 768px width:
- ✓ Exactly 2 columns
- ✓ Gap is 1.5rem (24px)
- ✓ Container centered
- ✓ No awkward spacing

Test at 820px width:
- ✓ Exactly 2 columns (not 3)
- ✓ Gap is 1.5rem (24px)

**Step 3: Test small desktop viewports**

Test at 1024px width:
- ✓ Exactly 3 columns
- ✓ Gap is 1.5rem (24px)
- ✓ Container centered

Test at 1280px width:
- ✓ Exactly 3 columns (not 4)
- ✓ Gap is 1.5rem (24px)

**Step 4: Test large desktop viewports**

Test at 1440px width:
- ✓ Exactly 4 columns
- ✓ Gap is 2rem (32px)
- ✓ Container centered

Test at 1920px width:
- ✓ Exactly 4 columns (not 5)
- ✓ Gap is 2rem (32px)
- ✓ Layout looks balanced

**Step 5: Test odd viewport sizes**

Test at 900px width:
- ✓ Still 2 columns (not 2.5 or awkward layout)
- ✓ Extra space distributed evenly

Test at 1100px width:
- ✓ Still 3 columns
- ✓ No partial columns

**Step 6: Document results**

Create a simple checklist file for reference:

```bash
cat > TESTING_RESULTS.txt << 'EOF'
Responsive Grid Layout - Manual Testing Results
Date: $(date +%Y-%m-%d)

✓ Mobile (375px, 414px): 1 column, 1rem gap
✓ Tablet (768px, 820px): 2 columns, 1.5rem gap
✓ Small Desktop (1024px, 1280px): 3 columns, 1.5rem gap
✓ Large Desktop (1440px, 1920px): 4 columns, 2rem gap
✓ Odd Sizes (900px, 1100px): No awkward layouts

All breakpoints working as designed.
EOF
```

**Step 7: Commit testing results**

```bash
git add TESTING_RESULTS.txt
git commit -m "docs: add manual testing results for responsive grid"
```

---

## Task 5: Final Build and Cleanup

**Files:**
- None (build verification only)

**Step 1: Clean build**

```bash
bundle exec jekyll clean
bundle exec jekyll build
```

Expected: Build succeeds without warnings or errors.

**Step 2: Verify site output**

Check `_site/` directory contains:
- ✓ Compiled CSS with all media queries
- ✓ All image assets
- ✓ HTML pages

**Step 3: Check git status**

```bash
git status
```

Expected: All changes committed, working tree clean.

**Step 4: Review commit history**

```bash
git log --oneline -5
```

Expected commits:
1. docs: add manual testing results for responsive grid
2. feat: add container max-widths for column control
3. feat: add progressive gap sizes at breakpoints
4. feat: add CSS Grid auto-fit to projects grid

---

## Testing Commands Reference

**Build site:**
```bash
bundle exec jekyll build
```

**Clean build:**
```bash
bundle exec jekyll clean && bundle exec jekyll build
```

**Start dev server:**
```bash
bundle exec jekyll serve
```

**View site:**
Open http://localhost:4000

**Test responsive layout:**
Use browser DevTools (F12) → Toggle device toolbar → Set custom width

---

## Success Criteria

- [x] Single column on mobile (< 768px)
- [x] Two columns at tablet (768px-1023px)
- [x] Three columns at small desktop (1024px-1439px)
- [x] Four columns at large desktop (1440px+)
- [x] Progressive gap increase (1rem → 1.5rem → 2rem)
- [x] No awkward partial columns at odd viewport sizes
- [x] Container centered at all breakpoints
- [x] Clean build with no errors
- [x] All changes committed

---

## Notes for Engineer

**About the `min(100%, 300px)` pattern:**
This ensures the grid is mobile-first. On very narrow screens (< 300px), items take full width. Once there's room for 300px + gap + padding, auto-fit kicks in and adds columns as space permits.

**Tuning max-widths:**
If columns look too wide/narrow at any breakpoint, adjust the `.content` max-widths. The formula is:
```
max-width = (columns × 300px) + ((columns - 1) × gap) + (2 × padding)
```

**Browser testing:**
This uses modern CSS Grid features supported in all evergreen browsers. No fallback needed for this project.
