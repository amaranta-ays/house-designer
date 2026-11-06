# Color Palette

## Tokens

| Token | Hex | Used for |
|---|---|---|
| `--color-fg` | `#0A0A0A` | Primary text on either background |
| `--color-fg-secondary` | `#374151` | Body secondary text |
| `--color-fg-tertiary` | `#6B7280` | Meta text, captions, labels |
| `--color-bg-warm` | `#FAFAF8` | Document and print background |
| `--color-bg-stark` | `#FFFFFF` | GUI / screen background |
| `--color-border-warm` | `#D6CDB8` | Hairlines on warm background |
| `--color-border-stark` | `#E5E7EB` | Hairlines on stark background |
| `--color-accent` | `#134E4A` | Single accent — Teal 900 |
| `--color-fg-inverted` | `#FAFAF8` | Primary text on inverted (near-black) surfaces |
| `--color-fg-secondary-inverted` | `#D1D5DB` | Secondary text on inverted surfaces |
| `--color-bg-emphasis` | `#0A1F2A` | Slide-emphasis surface (slides/HTML-presentations ONLY) |
| `--color-accent-bright` | `#4195BE` | Bright accent on slide-emphasis surfaces ONLY — a cyan-blue hue, NOT the `--color-accent` family (see the 2026-09-05 amendment) |
| `--surface-chrome-bg` | (consumer-defined) | Primary-surface chrome background. Set on the panel root; inherited by child modals/drawers/popovers |
| `--surface-chrome-fg` | (consumer-defined) | Primary text on the chrome (header titles, button labels at full strength) |
| `--surface-chrome-fg-secondary` | (consumer-defined) | Secondary text on the chrome (subtitles, default-state icon-button color) |
| `--surface-chrome-button-bg` | (consumer-defined) | Chrome-button fill (typically a translucent overlay matching the chrome theme) |
| `--surface-chrome-button-border` | (consumer-defined) | Chrome-button border (typically a slightly higher-opacity overlay than the button bg) |
| `--chip-border` | `var(--color-border-stark)` | Default chip outline color |
| `--chip-fg` | `var(--color-fg-secondary)` | Default chip text color |
| `--chip-bg` | `transparent` | Default chip fill (transparent by spec; surface themes do not override to a hue) |
| `--chip-border-style` | `solid` | Default chip outline style (state variants may override to dashed) |
| `--chip-current-border` | `var(--color-accent)` | `is-current` chip outline |
| `--chip-current-fg` | `var(--color-accent)` | `is-current` chip text |
| `--chip-done-border` | `var(--color-fg-tertiary)` | `is-done` chip outline (rendered dashed) |
| `--chip-done-fg` | `var(--color-fg-tertiary)` | `is-done` chip text |
| `--chip-success-border` | `#166534` | `is-success` chip outline (semantic carve-out) |
| `--chip-success-fg` | `#166534` | `is-success` chip text |
| `--chip-danger-border` | `#B91C1C` | `is-danger` chip outline (semantic carve-out) |
| `--chip-danger-fg` | `#B91C1C` | `is-danger` chip text |
| `--chip-warning-border` | `#B45309` | `is-warning` chip outline (semantic carve-out) |
| `--chip-warning-fg` | `#B45309` | `is-warning` chip text |

## Surface Rules

| Surface | Background token | Border token |
|---|---|---|
| Document / print | `--color-bg-warm` (`#FAFAF8`) | `--color-border-warm` |
| GUI / screen | `--color-bg-stark` (`#FFFFFF`) | `--color-border-stark` |
| Slide (light) | `--color-bg-stark` or `--color-bg-warm` per slide intent | `--color-border-stark` |
| Slide (inverted) | `#0A0A0A` (case-by-case for emphasis slides) | `#374151` — text uses `--color-fg-inverted` (`#FAFAF8`) primary and `--color-fg-secondary-inverted` (`#D1D5DB`) secondary |
| Slide (emphasis) | `--color-bg-emphasis` (`#0A1F2A`) — cyan-blue | Accent dividers use `--color-accent-bright` (`#4195BE`); text uses `--color-fg-inverted` primary and `--color-fg-secondary-inverted` secondary |

## Slide-Emphasis Carve-out (approved 2026-05-12)

The single-accent rule has **one approved exception**: slide / HTML-presentation surfaces may use the cyan-blue emphasis surface (`#0A1F2A`) paired with the bright cyan-blue accent (`#4195BE`). No other surface (GUI dashboards, documents, email, print) may use these tokens.

**This exception was a single-accent-*family* rule until 2026-09-05, and is no longer.** The pair was deep teal, chosen as a lightness sibling of `#134E4A`. It is now a different hue. See the amendment at the end of this section for what replaced the family justification and why.

**Why slides only:** Presentation surfaces have a different visual brief — they're shown briefly, at distance, often projected. Deep-teal-on-deep-teal carries the brand without the warm-white reference frame that document and screen surfaces rely on. The bright accent stays visible on the deep surface where the canonical `#134E4A` would disappear.

**How to apply (in `house-style-render`):**
- Wrapper class: `<div class="medium-slide emphasis house-style">…</div>`
- The `.emphasis` modifier locally overrides `--color-accent` to `var(--color-accent-bright)` so existing accent uses (italic emphasis, dividers, key-figures, callouts) automatically render in the brighter teal — no per-component overrides needed
- Pattern overlay (interlocking SVG, single-accent stroke at low opacity) is permitted on emphasis surfaces; see `frontend-slides` skill for the canonical pattern asset

**What this carve-out does NOT permit:**
- Using `#4195BE` on warm-white or stark-white surfaces — that's a single-accent-rule violation
- Using `#0A1F2A` as a "dark mode" for GUI dashboards or documents — those surfaces remain light-mode only
- A second accent hue (e.g. amber, coral) on emphasis slides — the family rule is one-hue, two-lightness

### Genre-Driven Deck Surface (amendment, approved 2026-09-04) — SUPERSEDED 2026-09-29

*Superseded by "Dark by Default" below. Kept as the record of what the rule was.*

The 2026-05-12 carve-out is extended from *a slide* to *a deck of a projected genre*. Its own stated rationale — presentation surfaces are "shown briefly, at distance, often projected" — describes an entire walkthrough, not one slide inside it, and does not describe a proposal PDF in an inbox at all.

A deck declares a `genre`. The surface is **derived** from it and is not a user setting:

| `genre` | Surface |
|---|---|
| `proposal` | Light — `--color-bg-stark` |
| `discovery` | Light — `--color-bg-stark` |
| `walkthrough` | Emphasis — `--color-bg-emphasis` + `--color-accent-bright` |
| `training` | Emphasis — `--color-bg-emphasis` + `--color-accent-bright` |

Light genres are printed, attached and forwarded, and must survive as a PDF. Emphasis genres are projected in a dim room and never leave the screen.

**This table is exhaustive.** Adding a genre means updating both this table and the `Genre` enum that deck-studio keeps at `deckstudio/model/genre.py`, which raises on an unknown genre rather than defaulting to a surface nobody chose. The classification test: is the deck read on a screen and forwarded, or projected in a room and then discarded?

**Why light decks use `--color-bg-stark` and not `--color-bg-warm`.** The Surface Rules table above permits either for a light slide, "per slide intent" — this records the intent. Stark white is what a deck is read against on a screen before it is ever printed, and it keeps a projected deck and a forwarded deck differing in surface alone rather than in surface and warmth together. Decided from rendered samples in the 2026-08-26 design session; see `ai-lab/docs/superpowers/specs/2026-09-04-deck-studio-design.md` §7.

**Consumer:** `deck-studio` (the `deck-studio` repo). No other surface may derive a background from a genre.

**This amendment does not relax any existing prohibition.** The three prohibitions listed above under the 2026-05-12 carve-out stand unchanged; this widens *when* the emphasis surface may be used, not *what* it permits.

### Dark by Default (amendment, approved 2026-09-29)

**A deck renders on the emphasis surface (dark) unless it asks for light.** Its genre plays no part. A deck asks in its meta: `surface: light`, or `surface: emphasis` to say dark explicitly. Absent means emphasis.

| deck meta | Surface |
|---|---|
| no `surface` | Emphasis — `--color-bg-emphasis` + `--color-accent-bright` |
| `surface: emphasis` | Emphasis — `--color-bg-emphasis` + `--color-accent-bright` |
| `surface: light` | Light — `--color-bg-stark` |

**Why.** This is the user's standing preference, stated 2026-09-29: decks are dark, and light only when specifically requested. Under the 2026-09-04 rule, `proposal` and `discovery` decks rendered light with no way to ask otherwise.

**The light surface keeps its reason; the reason now attaches to the request.** The 2026-09-04 rationale for light still holds for any deck the user wants light: decks that are printed, attached and forwarded must survive as a PDF in someone's inbox. It now attaches to `surface: light`, not to a genre. The choice of `--color-bg-stark` over `--color-bg-warm` for light decks is unchanged.

**Genre keeps every other job.** deck-studio still uses `genre` to decide which decks must cite their sources. **Never change a deck's genre to change its colour:** that silently changes what the deck is validated against.

**Consumer:** `deck-studio` only, as before. The rule lives in `Deck.surface` (`deckstudio/model/deck.py`), and the genre-to-surface mapping is deleted. The vocabulary stays `light` / `emphasis`: "dark" is how the user says `emphasis`, and `surface: dark` is refused rather than accepted as an alias.

**This amendment relaxes no prohibition.** The 2026-05-12 carve-out's three prohibitions stand unchanged. This changes *which decks* use the emphasis surface, not *what* the surface permits.

### Deck Footer in Mid Grey (amendment, approved 2026-10-23)

On slides, the footer's text and the brand mark both take `--color-fg-tertiary`. That is a step quieter than `--color-fg-secondary-inverted`, which the footer used before. The 3.5:1 contrast on the emphasis surface is an accepted departure for chrome. Full rule and rationale: [`density.md`](density.md#deck-cover-footer-and-rhythm-amendment-approved-2026-10-23).

### Emphasis Surface Hue (amendment, approved 2026-09-05)

The emphasis surface moved from deep teal (`#0A2A28` + `#3FB8A8`) to cyan-blue
(`#0A1F2A` + `#4195BE`). This is a value change to existing tokens, not a new
pair: nothing gains a token, and the teal pair is retired rather than kept
alongside.

**What this costs, stated plainly.** The 2026-05-12 carve-out justified itself
as a single-accent-**family** rule — the bright accent was a lightness sibling
of `#134E4A`, so the exception widened lightness but never hue. That
justification is gone. `#4195BE` is a different hue from `#134E4A`, and the
emphasis surface is now the one place in the house style where a second hue
appears. The surface and its accent remain a family with each other.

**Why the swap is otherwise safe.** The replacement pair holds the original's
saturation and lightness and rotates only hue, so weight and depth are
unchanged. The accent was then lightened until it cleared 5:1 against its own
surface; a constant-lightness rotation would have landed at 3.45:1, because
blue reads darker than teal at equal HSL lightness. Measured contrasts on the
new surface: body text 16.17:1, secondary text 11.47:1, accent 5.04:1 — the
accent clears AA at body size, which matters because italic emphasis is body
text, not a large-text exemption.

**Blast radius, audited before the change.** `--color-bg-emphasis` is consumed
only by `.medium-slide.emphasis` in `components.css`. No source file in
netsuite-, portfolio- or travel-command-center renders that class today; it
appears only inside NCC's bundled `dist` CSS, which is build output. The change
therefore alters no currently rendered surface other than deck-studio's own.

**What this amendment does NOT permit:**

- A third hue anywhere. The exception is one surface, one accent family.
- `#4195BE` on warm-white or stark-white — still a single-accent-rule violation.
- Reading this as licence to pick a deck's colours. The surface is derived from
  genre (see the 2026-09-04 amendment above) and is not an author setting.

## Accent Usage Map

The single accent `#134E4A` is permitted on **small surfaces only**.

| Element | Accent allowed? | Notes |
|---|---|---|
| Italic emphasis text | YES | The primary emphasis treatment |
| Button fill | YES | White text on accent fill |
| Divider rule (border-color, 1-2px) | YES | Section breaks, content separators |
| Link underline | YES | Use `text-underline-offset: 3px` |
| Key-figure numeric values (tabular) | YES | Highlight critical metrics |
| Small-caps section labels | YES | Optional accent on uppercase labels |
| Background fill (any size) | **NO** | Never as fill or gradient |
| Large headings (24px+) | **NO** | Black on background only |
| Hero / banner panels | **NO** | Never as backdrop |
| Body paragraph color | **NO** | Body stays foreground tokens only |

## Surface-Hierarchy Color Inheritance (approved 2026-05-13)

When a secondary surface (modal, drawer, popover) is launched from within a specific primary surface and is *scoped to that surface's context*, it inherits the primary's chrome color scheme: header background, header text and secondary-text colors, header-button styling.

### Authority model

- **Primary surfaces are theme authorities.** A primary surface is a top-level panel or section (e.g., a Project Detail panel, a Lab workspace, a NAS workspace). The primary surface owns its chrome theme — it MUST declare the five chrome variables (see below) on its root element. The value of each variable is the consumer's design choice; consumers can theme each primary surface independently.
- **Secondary surfaces inherit, never decide.** A secondary surface (modal, drawer, popover) launched from within a primary surface MUST read its chrome from CSS variables — never from color literals. This makes the inheritance mechanical: the secondary surface picks up whatever theme is in effect for its parent, no matter which primary surface launched it.

### The five canonical chrome slots

| Slot | Role |
|---|---|
| `--surface-chrome-bg` | Header / chrome background |
| `--surface-chrome-fg` | Primary text on chrome (titles, button labels) |
| `--surface-chrome-fg-secondary` | Secondary text on chrome (subtitles, icon-button default) |
| `--surface-chrome-button-bg` | Chrome-button fill (typically a translucent overlay) |
| `--surface-chrome-button-border` | Chrome-button border (typically a higher-opacity overlay than button-bg) |

Each primary surface in a consumer (e.g., `ProjectDetail` in NCC, `LabPanel` in NCC, `NasPanel` in NCC) defines values for all five slots on its outer container. Child surfaces consume them and look correct in every theme without code changes.

### Applies to

Child surfaces — secondary surfaces whose content derives from or operates on the parent surface's data. Examples:

- A "Status Brief" modal opened from a Project Detail panel inherits the panel's chrome theme.
- An Edit dialog launched from a row's action menu inherits its row's container chrome.
- A confirmation popover triggered by a button inside a project view inherits the surrounding chrome.

### Does NOT apply to

- **Independent surfaces** — global modals like Settings, Help, or system notifications. These use the default neutral chrome regardless of where they were launched, because they're triggered from any panel and inheriting from "whichever surface happened to launch me" would make them inconsistent across launch points.
- **Semantic-color overrides** — destructive-action confirmations (red), error banners, success states (green), warnings (amber). The signal color takes precedence over inheritance; the rule is about reducing arbitrary color variation, not suppressing meaningful color signals.

### Inheritance scope

Chrome only: header background, header text, header secondary-text shade, header-button states (default, hover, focus). Body content of the secondary surface follows the medium's body rules (e.g., warm white for documents), not the parent's chrome. The accent color (`--color-accent`) remains a system-wide constant under the single-accent rule and does not inherit per surface.

### Rationale

Child surfaces that visibly belong to their parent reduce context-switching cost. The user perceives the secondary surface as continuing the parent's task rather than as a new context. This is the single-accent rule applied to surface hierarchy: don't introduce a new color where you can inherit one.

### Required implementation

The cascade-based pattern is canonical and MUST be used:

1. **Primary surface declares the five chrome variables** on its outer container, either via inline style (when the values depend on per-instance state) or via a className that resolves to those variables (when the values are static for the surface type).
2. **The primary surface's render output must be a DOM element**, not a fragment (`<>`), so that sibling-rendered child modals/drawers/popovers are DOM descendants of the themed container. A fragment groups renders but creates no DOM node — variables declared on a fragment's first child don't cascade to its later siblings. If a primary surface needs to render multiple top-level elements (e.g., a panel + several modals), wrap them in a single themed `<div>`. This wrapper has no layout effect when its children use `position: fixed`.
3. **Child surfaces consume each slot via `var(--surface-chrome-*)`** in their styles — no color literals.
4. **Chrome buttons MUST use the canonical helper classes** (`.hsr-chrome-button` for solid-ish pill buttons, `.hsr-chrome-icon-button` for transparent icon-only buttons) from `house-style-render/shared/components.css`. These classes encapsulate the default, hover, focus, and disabled states using `color-mix()` against the chrome slots — they work uniformly across any theme polarity (light-on-dark, dark-on-light) without per-consumer hover math.

### Canonical chrome-button classes

| Class | Use for | Default state | Hover state |
|---|---|---|---|
| `.hsr-chrome-button` | Solid-ish pill buttons with visible bg + border (Copy, Print, Save) | `bg = --surface-chrome-button-bg`, `border = --surface-chrome-button-border`, `color = --surface-chrome-fg` | `bg` and `border` blended 60/40 toward `--surface-chrome-fg` via `color-mix()` |
| `.hsr-chrome-icon-button` | Transparent icon-only buttons (close X, archive, edit) | `bg = transparent`, `border = transparent`, `color = --surface-chrome-fg-secondary` | `color` shifts to `--surface-chrome-fg`, `border` reveals at `--surface-chrome-button-border` |

Both classes use `var(--transition-fast)` for state transitions, honor `prefers-reduced-motion`, and include `:focus-visible` styling that uses `--surface-chrome-fg` for accessible keyboard focus.

### Anti-patterns (will fail review)

- **Hardcoded color literal in a child surface.** If the parent's theme changes — or the same child is launched from a differently themed parent — a hardcoded child breaks the inheritance silently.
- **Re-implementing hover math in a consumer.** Patterns like `bg-[var(--surface-chrome-button-bg)] hover:bg-[some-other-color]` defeat the canonical classes; use `.hsr-chrome-button` so hover is consistent system-wide.
- **Fragment as the primary-surface render root.** Variables on the fragment's first child don't cascade to sibling-rendered modals; wrap in a themed `<div>`.

## Forbidden Palette

- Pure black `#000000` text (too harsh on warm-white surface)
- Purple / indigo gradients (`#5B5BE5` family, `#6366F1`, `#8B5CF6`)
- Pastel pill / chip backgrounds
- Multiple-color callout systems
- Neon / saturated brights of any hue
- Any second accent introduced "for variety"

## Chip Primitives (approved 2026-05-13)

Categorical chips (phase, type, complexity, tag, account, status) MUST use the canonical `.hsr-chip` primitive with state variants. Pastel-pill recipes (`bg-{hue}-100 text-{hue}-700`) and per-category color maps (e.g. `PHASE_COLORS`, `COMPLEXITY_COLORS`) are forbidden — they regress to multi-hue categorical hierarchy that the single-accent rule rejects.

### The canonical chip class

| Class | Variant | Role |
|---|---|---|
| `.hsr-chip` | default | Neutral outlined. Default for categorical: tag, type, complexity, account, generic attribute. |
| `.hsr-chip.is-current` | accent outlined | One-per-scope. Current phase, active filter, selected category. |
| `.hsr-chip.is-done` | muted dashed outlined | Completed / superseded / archived state in a multi-stage workflow. |
| `.hsr-chip.is-success` | green outlined | Semantic carve-out for success / complete signals. |
| `.hsr-chip.is-danger` | red outlined | Semantic carve-out for failure / error signals. |
| `.hsr-chip.is-warning` | amber outlined | Semantic carve-out for blocked / requires-attention signals. |

### Shape, weight, density (locked)

- Outlined pill (`border-radius: 999px`, no fill except in chrome polarity).
- Padding `3px 9px`. Internal text 10px Inter weight 500, uppercase, tracking 0.12em.
- Single line, never wraps.
- Optional leading icon (4–10px) gap 4px.

### Polarity behavior

Chip variants read from `--chip-*` tokens. Light-surface defaults are declared in `tokens.css` at `:root`. Surface-theme classes (in consumer CSS, scoped to the surface's root) override these tokens to flip polarity on dark chrome. Example for NCC's Lab and Project Detail panels:

```css
.surface-theme-lab,
.surface-theme-lab-chats,
.surface-theme-project-detail {
  --chip-border:         rgba(250, 250, 248, 0.30);
  --chip-fg:             var(--surface-chrome-fg-secondary);
  --chip-current-border: #5EEAD4;
  --chip-current-fg:     #5EEAD4;
  --chip-done-border:    rgba(250, 250, 248, 0.18);
  --chip-done-fg:        rgba(250, 250, 248, 0.45);
  --chip-success-border: #4ADE80;
  --chip-success-fg:     #86EFAC;
  --chip-danger-border:  #FCA5A5;
  --chip-danger-fg:      #FCA5A5;
  --chip-warning-border: #FCD34D;
  --chip-warning-fg:     #FDE68A;
}
```

The chip itself never knows what polarity it's in. The cascade does the work.

### Non-chip companions

Three primitives that look chip-adjacent but are not chips:

| Class | Role |
|---|---|
| `.hsr-mono-id` | Identifier in JetBrains Mono with a subtle background fill — atom refs, ticket IDs, project IDs. Reads as code, not chip. |
| `.hsr-label` | Inline small-caps label, no border / no fill — iteration counters ("Round 2"), build numbers. Optional `<span class="num">` inside renders the number in accent tabular numerics. |
| `.hsr-status-italic` | Italic-accent inline status with leading dot — still transients with no orbit ("No reason recorded", a stale-review note). Never paired with the orbit: a running status follows [`motion.md`](motion.md#orbit-with-a-label-one-colour-upright-approved-2026-11-06) (one colour with the orbit, upright, no dot). |

### Anti-patterns (will fail review)

- **Pastel-pill recipes.** Any `bg-{hue}-100 text-{hue}-700` Tailwind pattern is an anti-pattern. Even neutral pastels (slate-100) are wrong shape — chips are outlined, not filled, on the canonical light surface. *One exception exists and it is narrow:* printed output from `house-style-render`'s PDF and DOCX adapters, per carve-out #9 in [`house-style.md`](house-style.md) (approved 2026-09-09, amended v1.8.0), sets a timestamp as a filled Teal 900 badge with white text. It is not a licence for filled chips anywhere else — GUI, email and slide surfaces keep outlined chips, and nothing in the web or React layer inherits it. Since v1.8.0 the badge is the *default* of those two adapters rather than an opt-in; a caller that wants the older unfilled treatment asks for `PdfOptions.memo()`.
- **Per-category color maps.** `PHASE_COLORS`, `COMPLEXITY_COLORS`, or any record that maps a category value to a hex / utility-class pair. Hue is reserved for the single-accent role and the three semantic carve-outs; everything else differentiates via copy and state.
- **Decorative bold inside chips.** Chip text is weight 500, never 600/700. If a chip's content needs to read heavier, it's signaling at the wrong layer — promote to a section heading.
- **Hue as state.** Using `is-current` color outside of `is-current` semantics (e.g., "make this chip teal because the row is selected"). State is reserved for state.

### Migration notes

When migrating an existing surface:

1. Remove the per-category map entirely. The chip class doesn't need it.
2. At each call site, replace the dynamic class with `.hsr-chip` plus the appropriate state modifier. Most categorical chips become plain `.hsr-chip` — the copy carries the meaning.
3. For workflow phases: the *current* phase on a row is `.hsr-chip.is-current`. Other rows showing the same phase value are still `.hsr-chip.is-current` (each row's phase is "the current phase of that row"). For a phase TIMELINE within a single project view, use `.hsr-chip.is-done` for past phases, `.hsr-chip.is-current` for the active one, default for upcoming.
4. For pipeline status: `running` → the OrbitSpinner with an upright label in one colour with it (white on dark chrome; see [`motion.md`](motion.md#orbit-with-a-label-one-colour-upright-approved-2026-11-06), amended 2026-11-06 — this line said `.hsr-status-italic` until then); `complete` → `.hsr-chip.is-success`; `error` → `.hsr-chip.is-danger`; `blocked / awaiting inputs` → `.hsr-chip.is-warning` or `.hsr-status-italic` for transients.
5. For semantic distinctions that *aren't* one of the four built-in carve-outs (success/danger/warning/current): use copy. If you're reaching for a fifth color, you're solving the wrong problem.
