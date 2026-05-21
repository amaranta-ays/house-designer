# Slide Deck — Annotations

A 4-slide reference deck demonstrating the house style applied to the slide medium. Used by the agent for visual pattern-matching during critique.

## What's on each slide

1. **Title slide** (`#FFFFFF` stark white) — label + large dual-line title + italic subtitle
2. **Metric slide** — three-metric grid in JetBrains Mono, accent color on the big numbers
3. **Analysis slide** — body paragraph with italic-accent emphasis on the key figure
4. **End slide** (`#0A0A0A` inverted) — large dual-line statement in light-on-dark, accent on label

## Key token applications

| Element | Token / value | Reference |
|---|---|---|
| Display type | Inter weight 500, `clamp(40px, 6vw, 72px)`, tracking -0.03em | [`type-stack.md`](../type-stack.md) |
| Slide background (light) | `#FFFFFF` (stark white) | [`color.md`](../color.md) — screen surface |
| Slide background (inverted) | `#0A0A0A` for emphasis slides | [`color.md`](../color.md) |
| Body type | Inter weight 400, 18px, line-height 1.5 | [`type-stack.md`](../type-stack.md) |
| Section labels | JetBrains Mono weight 500, 12px uppercase, accent color | [`type-stack.md`](../type-stack.md) + [`color.md`](../color.md) |
| Metric numerals | JetBrains Mono weight 500, `clamp(40px, 5vw, 60px)`, accent color | [`type-stack.md`](../type-stack.md) |
| Italic emphasis | Inter italic in accent color | [`color.md`](../color.md) |
| Slide padding | `clamp(48px, 8vw, 96px)` per side | [`density.md`](../density.md) |

## What this example illustrates

1. Viewport-fit hard gate — every slide is `100vh`, no internal scroll.
2. Hierarchy from size alone — the title (72px) and body (18px) are 4 steps apart in the scale; weight is identical.
3. The inverted slide demonstrates that the system's accent (Teal 900) still functions on a near-black surface.
4. Metric grid (3 cards) is the maximum density permitted by the slide medium's limits.
5. Slide numbering is a hairline metadata element (JetBrains Mono 10px, secondary color).

## Common drift this example anchors against

- Hero gradient backgrounds (purple-to-indigo on title slide)
- Emoji icons in the metric grid captions
- Pastel chips on slide labels
- Bold weights creeping in for the metric numbers
- The accent appearing as a button-style chip background instead of as numeral color