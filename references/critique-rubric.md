# Critique Rubric

The enforcement mechanism for the critique-then-revise loop. Each line is a binary pass/fail check applied to every draft before declaring it done.

When a check fails, the agent revises the draft to address the failure. Maximum 2 revision passes; if rules still conflict after 2 passes, surface the conflict to the user.

**Scope:** Run only the categories applicable to the artifact's medium. Document checks don't apply to slides; print checks don't apply to GUI; motion checks don't apply to print.

---

## Type Rules

- [ ] Inter is the only sans-serif family in use (`font-family: 'Inter', ...`)
- [ ] No serif font families anywhere in the artifact (spec rejects serif)
- [ ] No third font family beyond Inter + JetBrains Mono
- [ ] Default weight ≤ 500 throughout
- [ ] No `font-weight: 600` or `font-weight: 700` unless user explicitly opted in (must be documented in artifact frontmatter or top-of-file comment)
- [ ] Tabular figures use JetBrains Mono with `font-feature-settings: "tnum"`
- [ ] Display text uses tracking `-0.025em` or `letter-spacing: -0.025em`
- [ ] Uppercase / small-caps labels use tracking ≥ 0.12em

## Color Rules

- [ ] Exactly one accent color in the entire artifact
- [ ] Accent hex matches `#134E4A` (Teal 900)
- [ ] No occurrence of `#000` or `#000000` in CSS or HTML (use `#0A0A0A`)
- [ ] Background hex matches the surface rule:
  - Document / print → `#FAFAF8`
  - GUI / screen → `#FFFFFF`
- [ ] No purple / indigo hexes (`#5B5BE5`, `#6366F1`, `#8B5CF6`, `#7C3AED` and family)
- [ ] No pastel pill backgrounds (light-saturated pinks, blues, greens, oranges)
- [ ] No multiple-color callout system
- [ ] Accent appears only on small surfaces (text emphasis, button fill, divider rule, link underline, key-figure values, small-caps labels) — never as background fill, gradient, or hero panel

## Density Rules

- [ ] All spacing values come from the 4px scale (4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 / 96 px)
- [ ] Document line length ≤ 75 characters (or 65ch for single-column body)
- [ ] Line-height matches the medium (1.6 doc / 1.55 GUI / 1.3-1.5 slide)
- [ ] Document outer margins ≥ 48px digital / ≥ 18mm print
- [ ] Slides: every slide fits one viewport, no internal scrollbars
- [ ] Slides: feature grids ≤ 6 cards, content slides ≤ 6 bullets, code slides ≤ 10 lines

## Hierarchy Rules

- [ ] Hierarchy is carried by size, color, space, and tracking — not weight
- [ ] Display tokens are ≥3 steps above body tokens in the size scale
- [ ] Body uses `#0A0A0A`; secondary text uses `#374151` or `#6B7280`
- [ ] Whitespace between major sections is 32–64 px
- [ ] Small-caps labels use uppercase + tracking, not the `font-variant: small-caps` feature (which depends on font support)

## Forbidden Patterns

- [ ] No `linear-gradient(...)`, `radial-gradient(...)`, or `conic-gradient(...)` anywhere
- [ ] No emoji glyphs as visual elements (text emoji in prose is acceptable; emoji-as-icon is not)
- [ ] No box-shadow beyond a single subtle layer (max: `0 1px 2px rgba(0,0,0,0.04)`)
- [ ] No `border-radius` > 12px
- [ ] No animation-iteration-count > 1 (no infinite loops)
- [ ] No parallax (no `transform: translate3d(...)` tied to scroll position)
- [ ] No bouncy / spring easings (e.g., `cubic-bezier(0.68, -0.55, 0.27, 1.55)`)

## Surface Match

- [ ] Background hex matches medium (warm vs. stark)
- [ ] Border hex matches background (warm border on warm bg, stark border on stark bg)
- [ ] Accent usage is appropriate for the surface (small surfaces only)

## Motion Rules (slides and GUI only)

- [ ] Slide decks include a navigation controller supporting keyboard (arrow keys / space / page-up / page-down), mouse wheel, and touch/swipe gestures; slide `height` uses both `100vh` and `100dvh` (the `dvh` fallback for mobile Safari)
- [ ] All transitions use one of the defined timing tokens (120ms / 240ms / 400ms)
- [ ] All transitions use one of the defined easing tokens (`ease-out`, `ease-in`, or default cubic-bezier)
- [ ] `prefers-reduced-motion: reduce` media query is honored (reduces all motion to ≤0.01ms)

## Print Rules (documents only)

- [ ] `@page` margins ≥ 18mm on all sides
- [ ] Print background matches the warm-white surface rule (`background: #FAFAF8` in `@media print`, not `background: white`)
- [ ] Renders without clipping at A4 page boundaries
- [ ] Renders without clipping at US Letter page boundaries
- [ ] Headers and footers (when present) use 10px JetBrains Mono for page numbers / Inter 400 for headers

---

## How to Use This Rubric

For each draft:

1. Identify the medium (document / slide / GUI).
2. Run the applicable categories above.
3. Report results per line: `PASS` or `FAIL` with cited line reference.
4. If any FAIL, revise the draft to address each failure, then re-run.
5. After 2 revision passes with remaining FAILs, surface the conflict to the user with a summary of which rules are in tension and why.

## How to Extend This Rubric

When a new failure pattern is discovered in production:

1. Add a new line to the appropriate category with a clear binary check.
2. If the pattern needs an example, add an entry to `anti-patterns.md` and reference it from the rubric line.
3. Commit the rubric change alongside the discovery.

The rubric is editable. The agent's enforcement updates the moment a new line is added.
