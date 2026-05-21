# Density and Spacing

## 4px Spacing Scale

All layout values come from this scale. No off-scale values permitted.

| Token | Value | Common uses |
|---|---|---|
| `--space-1` | 4px | Tight component padding, icon offsets |
| `--space-2` | 8px | Default component padding |
| `--space-3` | 12px | Card inner padding |
| `--space-4` | 16px | Default vertical rhythm |
| `--space-6` | 24px | Section gaps within a group |
| `--space-8` | 32px | Major section breaks |
| `--space-12` | 48px | Page-level vertical rhythm |
| `--space-16` | 64px | Hero / cover spacing |
| `--space-24` | 96px | Document outer margins (print) |

## Max Content Widths

| Context | Max width |
|---|---|
| Document body (single column) | 65ch (~700px at 16px) |
| Long-form report | 75ch |
| GUI section | 100% with internal `max-width: 1280px` |
| Slide | 100vw (viewport-fit hard gate) |

## Vertical Rhythm

All components anchor to an 8px baseline grid. Margins, padding, line-heights, and component heights must produce multiples of 8.

## Per-Medium Density Limits

### Documents
- Line length: max 75 characters
- Line-height: 1.6 body / 1.15 display
- Outer margins: ≥ 48px digital, ≥ 18mm print
- Section spacing: 32–48px between major sections
- Pull quotes may break out of the 65ch column width

### Slides
- **Viewport-fit is mandatory.** Every slide fits one viewport, no internal scroll.
- Max content per slide:
  - Title slide: 1 heading + 1 subtitle + 1 tagline
  - Content slide: 1 heading + 4–6 bullets OR 2 short paragraphs
  - Feature grid: max 6 cards
  - Code: max 10 lines, no horizontal scroll
  - Quote: 1 quote + 1 attribution
- Line-height: 1.3–1.4 display, 1.5 body
- Margins: 5–8% of viewport on all sides

### GUI / Screen
- Component padding: 4–8px scale only
- Minimum 8px between adjacent components
- Section spacing: 24–48px
- Line-height: 1.55 body, 1.2 display

## Print Specifications

- `@page` margins: minimum 18mm on all sides
- A4 and US Letter must both render without content clipping at page boundaries
- Page numbers (when present): bottom-right, 10px JetBrains Mono, accent color
- Headers (when present): top-left, 10px Inter weight 400, secondary text color

## Print CSS Boilerplate

```css
@page {
  size: A4;
  margin: 18mm;
}

@media print {
  body {
    background: #FAFAF8;
    color: #0A0A0A;
    font-family: 'Inter', sans-serif;
    font-weight: 400;
    font-size: 14px;
    line-height: 1.6;
  }
  .no-print { display: none; }
  a { color: #134E4A; text-decoration: underline; text-underline-offset: 2px; }
  h1, h2, h3 { page-break-after: avoid; }
}
```
