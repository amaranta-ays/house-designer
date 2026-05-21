# GUI Module — Annotations

A representative GUI module (card with data rows, footer with primary/secondary actions). Used by the agent for visual pattern-matching when working with `superpowers:frontend-design` on web app interfaces.

## Key token applications

| Element | Token / value | Reference |
|---|---|---|
| Module surface | `#FFFFFF` stark white, 1px hairline border, 8px radius, single subtle shadow | [`color.md`](../color.md) — screen surface |
| Footer surface | `#F9F9FB` (very subtle step from stark white) | Surface step without using the accent |
| Module title | Inter weight 500, 20px, with italic-accent emphasis on the quarter token | [`type-stack.md`](../type-stack.md) + [`color.md`](../color.md) |
| Meta label | JetBrains Mono weight 500, 10px uppercase, tertiary color | [`type-stack.md`](../type-stack.md) |
| Data row labels | Inter weight 400, secondary color | [`color.md`](../color.md) |
| Data row values | JetBrains Mono weight 500, tabular figures | [`type-stack.md`](../type-stack.md) |
| Primary metric value | Same as above but in accent color | [`color.md`](../color.md) — accent on small surface |
| Primary button | Accent background fill, stark-white text, 4px radius | [`color.md`](../color.md) — accent on small surface |
| Secondary button | Transparent with hairline border | Foreground tokens |
| Link underline | Accent color, offset 3px | [`color.md`](../color.md) |
| Hover transition | 120ms cubic-bezier(0,0,0.2,1) | [`motion.md`](../motion.md) |

## What this example illustrates

1. Stark white surface for screens (versus warm white for documents — the distinction is meaningful at this size).
2. Border radius is 8px — well within the 12px maximum, restrained.
3. Box shadow is `0 1px 2px rgba(0,0,0,0.04)` — single subtle layer, per the rule.
4. Primary action gets the accent fill (a "small surface" — buttons qualify). Other UI weight comes from position and chrome, not from additional colors.
5. Footer's slightly different background (`#F9F9FB`) creates visual separation without introducing a new color.
6. Italic emphasis in the title (`<em>Q3</em>`) uses accent color — typographic emphasis carries weight that bold would normally do.

## Common drift this example anchors against

- Border radius creeping up to 12-16px (overly rounded, friendly)
- Heavy box shadows (`0 4px 12px rgba(0,0,0,0.1)` or multi-layer)
- Pastel chips on the status / meta labels
- Bold weights on the data row values for "emphasis"
- Multiple accent colors (red for "down", green for "up")
- Gradient background on the primary button
