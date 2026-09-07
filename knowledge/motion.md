# Motion

## Timing Tokens

| Token | Value | Used for |
|---|---|---|
| `--motion-fast` | 120ms | Hover transitions, button feedback |
| `--motion-default` | 240ms | Reveal animations, state changes |
| `--motion-slow` | 400ms | Page-level transitions, modal entry |

## Easing Tokens

| Token | Value | Used for |
|---|---|---|
| `--ease-out` | `cubic-bezier(0, 0, 0.2, 1)` | Entries, fade-in, slide-up |
| `--ease-in` | `cubic-bezier(0.4, 0, 1, 1)` | Exits, fade-out |
| `--ease-default` | `cubic-bezier(0.4, 0, 0.2, 1)` | General transitions |

## Reveal Patterns

```css
.reveal {
  opacity: 0;
  transform: translateY(12px);
  transition: opacity 240ms cubic-bezier(0, 0, 0.2, 1),
              transform 240ms cubic-bezier(0, 0, 0.2, 1);
}
.reveal.in-view {
  opacity: 1;
  transform: translateY(0);
}
```

Trigger with Intersection Observer on scroll-into-view.

## Reduced-Motion Fallback

All transitions and animations must respect `prefers-reduced-motion`:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

## Forbidden Motion Patterns

- Parallax scrolling
- Infinite loops (rotating logos, pulsing dots, "live" effects) — except the orbit spinner carve-out below
- Spring physics / bouncy easings (e.g., `cubic-bezier(0.68, -0.55, 0.27, 1.55)`)
- Attention-seeking pulses — except the orbit spinner carve-out below
- Marquee / ticker scrolls
- "Wow" sequences on page load

Motion serves comprehension, not delight.

## Functional Async-State Carve-out (approved 2026-05-13)

The "infinite loops" and "attention-seeking pulses" prohibitions above apply to **decorative** motion. Motion that communicates **agent agency during async work** is functional, not decorative, and is permitted under one canonical pattern: the **orbit spinner**.

### The orbit spinner — the only sanctioned infinite-loop motion

| Property | Value |
|---|---|
| Geometry | 7-circle constellation: 1 center (r=3) + 6 outer (r=1.5) at clock positions 12/2/4/6/8/10 |
| Rotation | 360° clockwise, 1.5s linear, infinite — applied by the `.orbit` class |
| Per-circle opacity chase | Each outer circle pulses (base→1→base) on a 1.5s cycle, staggered by 0.25s per dot (= 1/6 of cycle) |
| Color | `fill="currentColor"` — parent's `color:` rule sets the hue |
| Reduced-motion fallback | `@media (prefers-reduced-motion: reduce)` halts both rotation and opacity-chase; static presence still signals async state |
| Canonical asset | [`../../plugins/house-style-render/shared/graphics/orbit-spinner.svg`](../../plugins/house-style-render/shared/graphics/orbit-spinner.svg) |
| CSS class | `.orbit` (rotation + size modifiers `.orbit-12` … `.orbit-48`) in [`../../plugins/house-style-render/shared/components.css`](../../plugins/house-style-render/shared/components.css) |
| React component | `<OrbitSpinner size={n} />` exported from `house-style-render-ts` |

### Sizing rule

| Context | Size | Color source |
|---|---|---|
| Inline in a button (e.g. "Rendering DOCX…") | 12px | `currentColor` (inherits from button) |
| Inside avatar bubble (chat thinking + streaming) | 16-18px in 28-32px bubble | white-on-accent |
| Standalone in chat message body | 20-24px | accent |
| "Loading-msg" row with text label | 20-24px | accent |
| Full-pane / empty-state loader | 32-48px | accent |

### Behavioral rule — same orbit, both states

The orbit covers both **thinking** (pre-stream, no output yet) and **streaming** (output flowing). The surrounding context differentiates the two — empty body in the thinking state, partial text in streaming — not the indicator itself. One idiom, one mental model.

### Why this is the only carve-out

The orbit is *information-carrying* (signals agent activity at known cycle rate) rather than *attention-extractive* (random pulses, gradient sweeps). It earns its motion budget by replacing prose ("the agent is thinking") with a recognizable shape. Any other "live" effect must justify itself the same way or it stays in the forbidden list.

### Mandatory placement (approved 2026-06-04)

The orbit spinner is not just permitted — it is **REQUIRED** whenever the UI initiates async work that takes longer than ~250ms and visibly delays the user's next action. The carve-out above grants permission; this rule asserts the obligation.

**When required:**

| Context | Pattern | Spinner size |
|---|---|---|
| Button triggers a network call, file op, or LLM run | Spinner inline beside the label; label transitions from action-name to gerund (`"Backfill"` → `"Backfilling…"`); button MUST be `disabled` while busy | 12-14px, `currentColor` |
| Data-fetching component during initial load | Spinner inline with the loading text label | 16-24px, accent |
| Search / filter input awaiting results (debounced or in-flight) | Swap the magnifier icon for a spinner inside the input chrome | 16px, parent fg |
| Long-running drawer / modal action (>3s, e.g. redistillation) | Spinner near the action button AND a textual ETA ("~3-5 min") | 14-16px |
| Background poll / passive refresh the user did not initiate | OPTIONAL — show only if interactive elements would mislead the user | 12px |

**Required pairing:**

The spinner alone is not sufficient. Every spinner placement MUST flip a *textual* label too — "Refresh now" → "Refreshing…", "Search" → "Searching…", "Save" → "Saving…". The label change keeps the state semantic for screen readers and reduces flicker confusion when the spinner alone is missed.

**Forbidden alternatives:**

- Text-only loading indicators (`"Refreshing…"`, `"Loading…"`) without an accompanying `OrbitSpinner` — the user has no visual proof the work is still in flight.
- Animated-text tricks (bouncing periods, marquee ellipses, CSS keyframes on dots) as a substitute for the spinner.
- Any third-party spinner library or framework default (MUI `CircularProgress`, Bootstrap `.spinner-border`, Tailwind `animate-spin` on a generic SVG, etc.) — the only sanctioned spinner is `<OrbitSpinner />` from `house-style-render-ts`.
- Showing the spinner *without* changing the label — semantic + visual must flip together.

**The 250ms heuristic:**

If the response can reliably return in <150ms (local SQL queries, in-memory state changes), the spinner is noise — omit it. If the user might click again or assume the action failed before the response arrives (typically >250ms perceived delay), the spinner is required. When in doubt, instrument the operation and decide from the p95 latency.

## Changelog
- **1.2.0 (2026-06-04):** Mandatory placement rule added — orbit spinner is now REQUIRED (not just permitted) during any user-initiated async work exceeding ~250ms. Codifies the placement table, the required label-transition pairing, the forbidden alternatives, and the 250ms duration heuristic.
- **1.1.0 (2026-05-13):** Added Functional Async-State Carve-out — orbit spinner approved as the cross-project agent thinking/streaming indicator.
