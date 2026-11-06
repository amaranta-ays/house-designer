# Type Stack

## Font Families

| Use | Family | Source | Fallbacks |
|---|---|---|---|
| Display, body | Inter | Google Fonts | `-apple-system, BlinkMacSystemFont, sans-serif` |
| Tabular figures, code | JetBrains Mono | Google Fonts | `'SF Mono', 'Cascadia Code', Consolas, monospace` |

### Print-mono divergence (decided 2026-09-09)

`house-style-render`'s **PDF and DOCX adapters set code spans in Courier / Courier New, not JetBrains Mono.** This is a decision, not an unfinished job — a reader who finds it and "corrects" it will reintroduce a bug.

The two media can't be satisfied by the same move. A PDF embeds a subset of whatever font it is handed, so vendoring JetBrains Mono would work there. A `.docx` only *names* fonts: OOXML supports embedding, but python-docx has the content-type constants and no part class that writes one, so the name is resolved by whichever machine opens the file. Naming a font that isn't installed is not a graceful degradation — Word silently substituted **Cambria, a serif**, for every timestamp on the machine this was built against, where JetBrains Mono is present only under Nerd Font family names (`JetBrainsMono NF`/`NFM`/`NFP`). Courier New resolves everywhere.

Vendoring for the PDF alone would leave the two adapters setting the same content in two different faces, which is worse than both being slightly off-spec. The cost is narrow: a code span in these documents carries times, identifiers and figures, all ASCII. Revisit if JetBrains Mono ever becomes reliably installable on the target machines, or if python-docx grows a font-embedding API.

The fallback chain above still applies to **web and GUI surfaces**, which are unaffected.

### DOCX body-font divergence (decided 2026-09-09)

`house-style-render`'s **DOCX adapter sets body text in Calibri, not Inter.** The PDF adapter stays on Inter. Same reasoning as the mono divergence above, applied to the body face — and it took two consumers overriding the default before the adapter was changed rather than each of them working around it.

A PDF **embeds** a subset of whatever font it is handed, so every reader of it sees Inter. A `.docx` only **names** a font, and Word resolves that name on whichever machine opens the file. Inter ships with no operating system: on the machine this was built against it was installed per-user only (`HKCU`, `AppData\Local\Microsoft\Windows\Fonts`), which means present for its author and absent for every recipient. Calibri ships with every Office install on Windows and Mac.

Naming Inter anyway and relying on fallbacks was tried and is recorded as insufficient. Empirical testing on 2026-05-11 (Word 16.0.20026, from the portfolio app's export module) found Word rendering **Times New Roman** for a missing font regardless of `w:altName` in `fontTable.xml`, a `minorFont` patched away from Cambria in the theme, or `w:rFonts` set at both run and Normal-style level. Those three hardening steps are still applied by the adapter — they moved there from the portfolio app on 2026-09-09 so every consumer gets them — but they are defence in depth. The font *name* is what decides the outcome.

The cost is that a day sheet downloaded as `.docx` is set in a different face from the same sheet downloaded as `.pdf`. That is worse than one face everywhere and better than a face nobody chose; the sizes, colour, spacing and layout stay identical. Revisit if OOXML font embedding becomes reachable from python-docx, or if Inter is installed machine-wide for every reader who receives these documents.

**Web font loading (HTML head):**

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

Note: only weights 400 and 500 are loaded by default. Adding 600/700 requires an explicit user opt-in (documented in the artifact's frontmatter or top-of-file comment).

### Slide-Typeface Carve-out (approved 2026-09-05)

`deck-studio`'s generated decks are set in **Calibri**, not Inter.

| Use | Family | Token | Fallbacks |
|---|---|---|---|
| deck-studio decks | Calibri | `font.sans_slide` | `Carlito, 'Segoe UI', sans-serif` |

**Scope, narrowly.** This applies to decks produced by `deck-studio`
(the `deck-studio` repo) and nothing else. GUI, document, email, print and
CLI stay on Inter, and so does the older `.medium-slide` path in
`components.css` — that class is not what deck-studio renders. Two slide
renderers therefore differ in typeface; if `.medium-slide` is ever retired in
favour of deck-studio, this row should absorb it rather than persist beside it.

**Size does not transfer between typefaces.** Calibri's x-height is 0.469 of its
nominal size where Inter's is 0.547, so the same `font-size` reads about 14%
smaller. deck-studio carries a single `--ds-type-scale` multiplier, **0.856**
since 2026-09-28, which is two factors:

- **1.07, derived:** 0.92 x 0.547 / 0.469, a deliberate 8% optical reduction
  expressed against Calibri's metrics. It looks like an enlargement and is
  not. Matching cap heights instead would give 1.055; x-height was chosen
  because it governs body text, where legibility is decided.
- **0.80, judged:** the user reviewed real decks at 1.07, found every size too
  large, and chose -20% from rendered samples over -35%.

On top of the scale, three headline roles carry their own factor, also set by
eye on 2026-09-28: statement text x0.55, divider titles x0.5, ordinary slide
headlines x0.72. Quote text shares the headline's type step and keeps the
base scale. Rendered at 1920x1080: quote 59px, statement 51px, divider 46px,
headline 43px, body 30px.

**Anything that changes the deck typeface must re-derive the 1.07 factor --
and only that one.** Carrying it onto a different face silently rescales every
deck; the 0.80 and the role factors are about how large a deck should look,
whatever its face.

**Portability, and why Carlito sits in the stack (amended 2026-09-05).** Calibri
ships with Windows and Office and is absent from most Macs and Linux machines.
It is also **proprietary and must never be embedded** — a deck goes to clients,
and embedding it would be redistributing a Microsoft font.

So the fallback names **Carlito**, which deck-studio vendors at
`deckstudio/fonts/` under the SIL OFL and embeds as `@font-face` on export,
behind `local('Calibri')`. Carlito is metric-compatible with Calibri, which is
what keeps the 1.07 factor above correct on a machine that has neither font
installed. A viewer with Calibri uses it and never downloads the payload;
everyone else gets metric-identical bytes we are licensed to ship.

Inter has the same portability problem for a different reason — it is not a
system font anywhere — and is not embedded, because no medium other than decks
exports a self-contained file.

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
