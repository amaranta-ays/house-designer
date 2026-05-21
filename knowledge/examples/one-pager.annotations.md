# One-Pager — Annotations

Canonical reference rendering. Used by the agent for visual pattern-matching during critique.

## Key token applications

| Element | Token / value | Reference |
|---|---|---|
| Layout grid | 2-column, 32px gap, max-width 800px | [`density.md`](../density.md) — 4px scale |
| Key-metric value | JetBrains Mono weight 500, 48px, accent color | [`type-stack.md`](../type-stack.md) + [`color.md`](../color.md) |
| Title | Inter weight 500, 32px, tracking -0.025em | [`type-stack.md`](../type-stack.md) |
| Subtitle | Inter weight 400 italic, 20px, secondary color | [`type-stack.md`](../type-stack.md) — italic emphasis |
| Section labels | JetBrains Mono weight 500, 10px uppercase, accent | [`color.md`](../color.md) — accent on small surfaces |
| Decision box | 1px accent border, full-width spanning both columns | [`color.md`](../color.md) |
| Next-steps numerals | JetBrains Mono with decimal-leading-zero counter, accent color | [`type-stack.md`](../type-stack.md) — tabular figures |
| Footer rule | 1px warm-white border above footer | [`color.md`](../color.md) |
| Background | `#FAFAF8` (warm white) | [`color.md`](../color.md) |

## What this example illustrates

1. Two-column dense layout fitting one printed page / one viewport.
2. Key metric as a large tabular numeric in accent — the highest-attention element.
3. The decision box spans both columns to anchor the page's purpose visually.
4. Numbered next-steps use `decimal-leading-zero` counters in JetBrains Mono — small typographic detail that signals editorial care.
5. Footer is hairline-separated, lowercase via JetBrains Mono uppercase tracking, contains only metadata (date / owner / version).

## Common drift this example anchors against

- The key-metric numeral gets bolded instead of using JetBrains Mono weight 500
- The decision box becomes a pastel pill / chip
- Numbered steps lose their tabular alignment (no JetBrains Mono)
- The two-column grid becomes a three-card grid (drift toward SaaS template)
