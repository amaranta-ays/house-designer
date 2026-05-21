# Board Memo — Annotations

This is a canonical reference rendering. The agent uses this file during critique to visually confirm that a draft "looks like the house style."

## Key token applications

| Element | Token / value | Reference |
|---|---|---|
| Body type | Inter weight 400, 14px, line-height 1.6 | [`type-stack.md`](../type-stack.md) |
| Display type | Inter weight 500, 32px, tracking -0.025em | [`type-stack.md`](../type-stack.md) |
| Heading-block labels | Inter weight 500, 10px uppercase, tracking 0.12em, JetBrains Mono family | [`type-stack.md`](../type-stack.md) |
| Executive summary | Inter weight 400 italic, 20px, accent color `#134E4A` | [`color.md`](../color.md) — accent on italic emphasis |
| Recommendation box | 1px border in accent, 24px padding | [`color.md`](../color.md) — accent on small surface |
| Signature rule | 2px solid accent above name | [`color.md`](../color.md) |
| Background | `#FAFAF8` (warm white) | [`color.md`](../color.md) — document surface rule |
| Print margins | `@page { margin: 18mm; }` | [`density.md`](../density.md) — print specifications |

## What this example illustrates

1. Hierarchy without weight contrast — the title (32px) is dramatically larger than the body (14px); weight is identical (500 vs. 400, single step).
2. Accent rationed to small surfaces only — italic emphasis, recommendation box border, signature rule, divider lines. Never as a fill.
3. JetBrains Mono used for the heading-block labels (TO/FROM/DATE/RE) — labels are a "tabular" treatment.
4. The recommendation box is the most visually weighted element via its accent border — using surface treatment, not type weight, to direct attention.
5. Warm white `#FAFAF8` reads as paper, not screen.

## Common drift this example anchors against

If a draft starts to look "off," compare against this rendering and check:
- Did the background drift to stark white?
- Did bold weights creep in for emphasis?
- Did a second accent appear (e.g., a red for the recommendation)?
- Did the type families drift (a different sans for headings, a serif for the title)?
- Did emoji or icons appear in the heading block?
