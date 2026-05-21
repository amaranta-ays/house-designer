# Type Stack

## Font Families

| Use | Family | Source | Fallbacks |
|---|---|---|---|
| Display, body | Inter | Google Fonts | `-apple-system, BlinkMacSystemFont, sans-serif` |
| Tabular figures, code | JetBrains Mono | Google Fonts | `'SF Mono', 'Cascadia Code', Consolas, monospace` |

**Web font loading (HTML head):**

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

Note: only weights 400 and 500 are loaded by default. Adding 600/700 requires an explicit user opt-in (documented in the artifact's frontmatter or top-of-file comment).

## Weight Usage

| Weight | Permitted use |
|---|---|
| 400 | Body, secondary text, italic emphasis (chat/email/slide mediums) |
| 500 | Display, headings, labels, button text, key-figure values |
| 600 | **Reserved.** Requires explicit user opt-in. |
| 700 | **Reserved** except for document-medium emphasis (`.medium-document em`) per v1.4.0 carve-out. Otherwise requires explicit user opt-in. |

The 600/700 reservation is the cornerstone of the "no bold default" rule — bold is not for decoration. The v1.4.0 carve-out unlocks 700 specifically for emphasis on operational documents (status briefs, memos), where italic at body size is too subtle to function as a scan target. Bold is still forbidden as decorative styling on headings, body paragraphs, or list items. The agent's critique-rubric enforces this.

## Size Scale (px)

| Token | Value | Used for |
|---|---|---|
| `--type-display-xl` | 48 | Hero / cover headlines |
| `--type-display-l` | 32 | Section headlines |
| `--type-display-m` | 24 | Subsection headlines |
| `--type-display-s` | 20 | Card titles |
| `--type-body` | 16 | Body text (GUI / screens) |
| `--type-body-doc` | 14 | Body text (documents / print) |
| `--type-meta` | 12 | Meta / secondary text |
| `--type-label` | 10 | Small caps labels (uppercase + tracking) |

For responsive scaling, use `clamp()`. Example for display-l: `clamp(24px, 4vw, 32px)`.

## Tracking (letter-spacing)

| Context | Value |
|---|---|
| Display (24px+) | `-0.025em` |
| Body | `-0.01em` |
| Small caps / uppercase labels | `0.12em` (or 1.6px+ for label-size text) |
| Tabular numerics | `-0.02em` |

## Line-Height per Medium

| Medium | Body line-height | Display line-height |
|---|---|---|
| Document / print | 1.6 | 1.15 |
| GUI / screen | 1.55 | 1.2 |
| Slide (display) | 1.5 | 1.3 |
| Slide (dense content) | 1.4 | — |

## Hierarchy Without Weight

Since 600/700 are reserved, hierarchy must come from other axes:

1. **Size scale (primary):** Display tokens are ≥3 steps above body tokens (e.g., 24px display vs. 14px body = 10px separation; 32px display vs. 14px body = 18px separation). The gap is intentionally large.
2. **Color contrast:** Body uses `#0A0A0A`; secondary text uses `#374151` (slate-700) or `#6B7280` (slate-500). Drop-off between hierarchies is carried by color step, not weight step.
3. **Whitespace:** 32–64 px between major sections; 16–24 px between related blocks; 8–12 px within a block.
4. **Tracking:** Uppercase labels (10–12 px with 0.12em tracking) feel hierarchical despite small size.
5. **Accent application:** Teal 900 on rules, italic emphasis, and key-figure values creates micro-hierarchy within a section.

## Tabular Figures

All numeric values (percentages, dates, currency, scores, statistics) MUST use JetBrains Mono with `font-feature-settings: "tnum"`:

```css
.tabular {
  font-family: 'JetBrains Mono', 'SF Mono', Consolas, monospace;
  font-feature-settings: "tnum";
  font-weight: 400;
}
```

Tabular figures may use weight 500 for emphasis (key metrics).

## Emphasis

Emphasis treatment is per-medium since v1.4.0 (2026-05-13):

| Medium | `<em>` treatment | Why |
|---|---|---|
| `.medium-gui` (chat) | Inter italic, weight 400, accent color | Reading-flow surface; italic preserves prose rhythm |
| `.medium-email` | Inter italic, weight 400, accent color | Reading-flow surface; italic matches document literary tone |
| `.medium-slide` | Inter italic, weight 400, accent color | Presentation surface; italic carries emphasis without competing with display weights |
| `.medium-document` (briefings, memos, status briefs, one-pagers) | Inter **bold 700**, non-italic, accent color | Operational documents are scanned more than read; italic at 12px body is too subtle to function as a scan target. Bold 700 reliably surfaces load-bearing facts (dates, owners, status counts) |

The accent color (`#134E4A`) applies in both treatments. The only differences across mediums are `font-style` (italic vs normal) and `font-weight` (400 vs 700). Single accent rule still holds — emphasis color is the system accent regardless of medium.

Use emphasis for:
- Inline emphasis on load-bearing facts (replacing what bold would do in conventional writing systems for chat/email/slide; matched directly to bold in document medium)
- Pull quotes (italic in all mediums; document-medium pull quotes use italic + accent + larger size, not bold)
- Section openers in dense paragraphs (e.g., `*Core manufacturing framework*` to anchor a topic shift)
- Status indicators in table cells (e.g., `*1 day overdue*` in document-medium tables renders bold)

Bold (700) remains forbidden as decoration. The carve-out is specifically for the `<em>` semantic role on document-medium surfaces — not a license for `**bold**` in markdown source (which the preprocessor continues to strip). Authors write `*emphasis*` (italic markdown) regardless of medium; the renderer paints it correctly per the table above.

## Tables

Tables inherit the body type stack — Inter, body size for the medium, body line-height. Browser UA stylesheets often reset `font-family` on `<table>`/`<th>`/`<td>`, breaking inheritance from the surface root, so the renderer sets `font-family` explicitly on each.

Header (`<th>`) treatment varies by medium:

| Medium | `<th>` size | `<th>` case | `<th>` weight | Rationale |
|---|---|---|---|---|
| GUI / dashboard (default) | 10px label | Uppercase + 0.12em tracking | 500 | Columns repeat across many rows; small-caps labels scan as labels |
| Document (memo, briefing, one-pager) | Body size | Sentence case | 500 | Prose-embedded tables; headers must sit inside reading rhythm, not interrupt it |
| Email | Body size | Sentence case | 500 | Email content is overwhelmingly prose-with-detail, not lookup grids; matches document treatment |
| Slide | *Not yet decided.* Inherits the default GUI treatment. |  |  |  |

The single-row-of-uppercase-labels treatment is the right choice when the table is a lookup grid (many rows of structured data scanned by column). It is the wrong choice when the table is *prose with a few rows of structured detail*, because the uppercase label row reads as a hard visual break in the reading flow. Slide tables are not yet decided — slides can carry both prose and lookup uses, and the call should follow a real-world example.

## Chips and labels

Categorical chips (`.hsr-chip`) and inline labels (`.hsr-label`) share the small-caps label treatment as their canonical typography:

| Role | Family | Size | Weight | Case | Tracking |
|---|---|---|---|---|---|
| `.hsr-chip` (all variants) | Inter | 10px | 500 | Uppercase | 0.12em |
| `.hsr-label` (inline label) | JetBrains Mono | 10px | 500 | Uppercase | 0.12em |
| `.hsr-label .num` (tabular number) | JetBrains Mono | 10px | 500 (inherits) | n/a | -0.02em + `tnum` |
| `.hsr-mono-id` (identifier) | JetBrains Mono | 10px | 500 | n/a (mixed case) | 0 + `tnum` |
| `.hsr-status-italic` (transient) | Inter | 11px | 400 | Sentence case | -0.01em + italic |

The chip family weight (500) is the only place at 10px where weight 500 is used purposefully — it's what makes uppercase + tight tracking still legible at small size. Increasing chip weight to 600/700 is an anti-pattern (covered in [`color.md`](color.md#chip-primitives-approved-2026-05-13)).

See [`color.md`](color.md#chip-primitives-approved-2026-05-13) for the full primitive catalog (variants, polarity, and migration notes from pastel-pill patterns to canonical chips).
