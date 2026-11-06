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

### Deck Cover, Footer and Rhythm (amendment, approved 2026-10-23)

Decided one rendered sample at a time while building a client discovery deck, then built into `deck-studio` (commits `18bf0ca`, `3c8cc98`, `d4f910b`, `b3199ca`). Every value below is in `cqw` of the slide, so it holds at any projection size.

**Cover.** A deck opens with a `title` slide: the client as a small uppercase eyebrow in the accent, the title as the largest type in the deck, the accent rule, the subtitle in body type, the date in meta type. Client and date come from the deck's meta, never retyped. **The cover carries no footer** — the client is already on it, and the logo and page number belong to the slides it opens. The cover counts as page 1, so the next slide reads `02`.

**Footer.** Three parts on one line: client name left, brand mark centred, page number right. **All three in the mid grey `--color-fg-tertiary`**, so the footer reads as one quiet band. On the emphasis surface that grey is 3.5:1 — below WCAG's 4.5:1 for small text, accepted deliberately for chrome meant to recede; on `light` it is 4.8:1. If a deck is shown somewhere the footer must be legible from a distance, lighten the text, not the mark.

**Brand mark.** Opt-in per deck (`brand:` names it); a deck with no brand carries no mark. The brand mark without its tagline, as a **single-colour mask painted `--color-fg-tertiary`** — never the two-colour white/yellow or black artwork, and never at reduced opacity. Height 2.17cqw (the lettering size approved with the tagline still attached). Embedded once per deck.

**Space at the headline rule.** The visible space under the rule equals the space over it, **measured as glyph ink, not box edges**: margin 2cqw above, 2.14cqw below. Equal margins read tight below, because a headline puts less leading under its baseline than a body line puts over its capitals.

**Compare column titles.** 1.6cqw between a column title and its first bullet, so the label reads as a heading over its list. The columns start 0.36cqw lower than a plain list, because a small uppercase label carries extra leading above its capitals; this puts their first ink level with a bullet slide's.

**Bullets.** Every bullet list is spaced alike: 1.2cqw between items, on bullet slides and compare columns both. One rule serves both, so they cannot drift apart.

**How these were decided.** Each was a request made while reviewing a rendered deck, checked against a rendered sample or by measurement before it was built. Spacing requests of the form "make X match Y" are settled by measuring rendered ink in the browser, not by equating CSS values.

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
