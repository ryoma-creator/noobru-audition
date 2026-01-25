# Color Tokens - Fixed Reference

**IMPORTANT: These colors are FIXED and must NOT be changed when implementing new sections or reading colors from images.**

## Brand Colors

- `--brand-yellow: #f9c42d` - Card offer backgrounds (yellow)
- `--brand-green: #63aa47` - Best Value header background
- `--brand-green-cta: #0fc270` - CTA button background
- `--brand-red: #e70e0e` - Most Popular header, price strikethrough, savings text

## UI Colors

- `--ui-bg-soft: #ffe9a9` - Soft background (for future sections)
- `--ui-border-gray: #676767` - Basic header background, borders
- `--ui-text-dark: #333333` - Dark text/button backgrounds (for future sections)

## Usage Rules

1. **Always use these tokens** - Never hardcode colors or read colors from images
2. **Do not change** - These are fixed brand colors, not to be modified
3. **Apply to all sections** - Use these same colors when implementing:
   - Guarantee Section
   - Reviews Summary Section
   - Reviews List Section
   - Any future sections

## Current Usage

- `.best-value-header`: `var(--brand-green)`
- `.most-popular-header`: `var(--brand-red)`
- `.card-offer`: `var(--brand-yellow)`
- `.basic-header`: `var(--ui-border-gray)`
- `.cta-button`: `var(--brand-green-cta)`
- `.price-was` (text-decoration-color): `var(--brand-red)`
- `.price-save`, `.save-amount`: `var(--brand-red)`
