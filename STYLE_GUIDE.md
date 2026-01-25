# Style Guide - Noobru Audition

**This document defines all design tokens, typography, spacing, and styling conventions used across all sections.**

## Design Principles

- **Mobile-first**: All styles start from mobile (380px) and scale up
- **Responsive scaling**: Use `clamp()` for fluid typography and spacing
- **Consistent tokens**: Always use CSS variables, never hardcode values
- **No animations**: Keep it simple, no transitions/animations unless explicitly requested

---

## CSS Variables

### Colors (Fixed - Do NOT change)

```css
--benefits-bg: #FFF9E6;        /* Light yellow background for benefits */
--brand-yellow: #f9c42d;        /* Card offer backgrounds */
--brand-green: #63aa47;        /* Best Value header */
--brand-green-cta: #0fc270;   /* CTA button background */
--brand-red: #e70e0e;          /* Most Popular header, strikethrough, savings */
--ui-bg-soft: #ffe9a9;         /* Soft background (banners, etc.) */
--ui-border-gray: #676767;     /* Basic header, borders */
--ui-text-dark: #333333;       /* Dark text/button backgrounds */
```

**Rule**: Always use these tokens. Never read colors from images or hardcode hex values.

### Spacing (Responsive)

```css
--space-0: 0;
--space-xs: clamp(0.25rem, 0.25vw + 0.125rem, 0.375rem);  /* 4-6px */
--space-1: clamp(0.5rem, 0.5vw + 0.25rem, 0.75rem);        /* 8-12px */
--space-2: clamp(1rem, 1vw + 0.5rem, 1.5rem);             /* 16-24px */
--space-3: clamp(1.5rem, 1.5vw + 0.75rem, 2.25rem);      /* 24-36px */
--space-4: clamp(2rem, 2vw + 1rem, 3rem);                 /* 32-48px */
```

**Usage**: Use `var(--space-X)` for all padding, margins, and gaps.

---

## Typography

### Font Family
```css
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
```

### Base Typography
```css
body {
    line-height: 1.6;
    color: #000000;
}
```

### Typography Scale (Mobile → Desktop)

#### Headings

**H1 / Main Title**
```css
font-size: clamp(28px, 5vw, 40px);
font-weight: 700;
line-height: 1.1;
```

**H2 / Section Labels (Card Headers)**
```css
font-size: clamp(24px, 5vw, 36px);  /* Desktop: 28px */
font-weight: 700;
line-height: 1.15;
color: #ffffff;  /* On colored headers */
```

**H3 / Offer Titles**
```css
font-size: clamp(26px, 5vw, 36px);  /* Desktop: 28px */
font-weight: 700;
line-height: 1.2;
color: #000000;
```

#### Body Text

**Subtitle**
```css
font-size: clamp(14px, 3vw, 18px);
color: #000000;
```

**Regular Text / Benefits**
```css
font-size: 14px;
color: #000000;
```

**Small Text / Banner**
```css
font-size: 14px;
color: #000000;
```

**Offer Subtext**
```css
font-size: 16px;
font-weight: 700;
color: #000000;
```

#### Pricing

**Supply Days / Price Each**
```css
font-size: clamp(28px, 5vw, 36px);  /* Desktop: 28px */
font-weight: 700;
line-height: 1.1;
color: #000000;
```

**Total Price**
```css
font-size: 20px;
font-weight: 400;  /* Label */
font-weight: 700;  /* Price value */
color: #000000;
```

**Savings / Discount**
```css
font-size: 20px;
font-weight: 400;
color: var(--brand-red);
```

**Impact Statement**
```css
font-size: 14px;
font-weight: 400;  /* Mobile */
font-weight: 700;  /* Desktop */
color: #000000;
```

---

## Breakpoints

### Mobile
- **Base**: 380px (mobile-first)
- **No media query needed** - base styles are mobile

### Desktop
- **Breakpoint**: `@media (min-width: 1200px)`
- **Container max-width**: `1200px`
- **Grid columns**: Use CSS Grid with `repeat(3, minmax(0, 1fr))` for 3-column layouts

---

## Layout Patterns

### Container
```css
.pricing-container {
    max-width: 1200px;
    margin: 0 auto;
}
```

### Section Padding
```css
/* Mobile */
padding: var(--space-2);

/* Desktop */
padding: var(--space-4);
```

### Card Layout (Desktop)
```css
.pricing-cards {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: var(--space-4);
    align-items: stretch;
}
```

### Card Styling
```css
.pricing-card {
    background-color: #ffffff;
    border: 2px solid var(--brand-yellow);
    border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);  /* Desktop only */
}
```

---

## Component Patterns

### Headers (Colored)
```css
.card-header {
    padding-block: var(--space-1);
    padding-inline: var(--space-3);
    text-align: center;
    border-radius: 12px 12px 0 0;
}

.best-value-header { background-color: var(--brand-green); }
.most-popular-header { background-color: var(--brand-red); }
.basic-header { background-color: var(--ui-border-gray); }
```

### CTA Buttons
```css
.cta-button {
    width: calc(100% - calc(var(--space-3) * 2));
    margin: 0 var(--space-3) var(--space-2) var(--space-3);
    padding: var(--space-2);
    background-color: var(--brand-green-cta);
    color: #ffffff;
    border: none;
    border-radius: 8px;
    font-size: 22px;
    font-weight: 700;
    white-space: nowrap;  /* Desktop */
}
```

### Benefits Box
```css
.additional-benefits {
    background-color: var(--benefits-bg);
    padding: var(--space-1) var(--space-2);
    border-radius: 8px;
    font-size: 14px;
    color: #000000;
}
```

---

## Desktop-Specific Adjustments

When implementing desktop styles (`@media (min-width: 1200px)`), apply these patterns:

### Typography Reductions
- **H2 (plan-label)**: `28px` (from clamp max 36px)
- **H3 (plan-offer-title)**: `28px` (from clamp max 36px)
- **Supply Days / Price Each**: `28px` (from clamp max 36px)

### Spacing Tightening
- Reduce vertical margins in pricing blocks
- Use `var(--space-xs)` for tight spacing between price elements

### Line Breaks
- Prevent wrapping: `white-space: nowrap` on desktop for:
  - CTA buttons
  - Impact statements
  - Price elements
  - Offer titles

---

## Usage Rules for New Sections

1. **Always use CSS variables** - Never hardcode colors or spacing
2. **Follow typography scale** - Use the defined font sizes and weights
3. **Mobile-first** - Write base styles for mobile, then add desktop media queries
4. **Use clamp()** - For responsive typography and spacing
5. **Consistent spacing** - Use `var(--space-X)` tokens
6. **No animations** - Unless explicitly requested
7. **Match existing patterns** - Follow the same structure as pricing section

---

## Example: Creating a New Section

```css
/* Mobile-first base styles */
.new-section {
    padding: var(--space-2);
}

.new-section-title {
    font-size: clamp(28px, 5vw, 40px);
    font-weight: 700;
    line-height: 1.1;
    color: #000000;
    margin-bottom: var(--space-2);
}

/* Desktop adjustments */
@media (min-width: 1200px) {
    .new-section {
        padding: var(--space-4);
    }
    
    .new-section-title {
        /* Additional desktop styles if needed */
    }
}
```

---

## Notes

- All prices use `data-variant` attributes (see `docs/price-script.md`)
- Images use Cloudinary with `f_auto,q_auto` parameters
- No external CSS frameworks or libraries
- Vanilla JavaScript only
