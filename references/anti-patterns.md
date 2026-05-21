# Anti-Patterns

Concrete examples of what to refuse, paired with compliant alternatives. When a draft (or a user request) matches one of these patterns, the agent declines, names the rule violated, and offers the corresponding alternative.

---

## Forbidden: Gradients

**Don't:**
```css
background: linear-gradient(135deg, #5B5BE5 0%, #8B5CF6 100%);
```

**Do (compliant):** Flat backgrounds only. Surface follows the medium rule (#FAFAF8 for documents, #FFFFFF for GUI). If you need visual distinction, use a hairline border or subtle background-color step (#F9F9FB on stark white) — never gradients.

**Rule violated:** house-style.md Forbidden Patterns ("Gradients of any kind").

---

## Forbidden: Multiple accent colors

**Don't:**
```css
.primary-action { background: #134E4A; }
.warning-action { background: #B45309; }
.danger-action { background: #DC2626; }
```

**Do (compliant):** One accent. Differentiate actions by shape, position, or surrounding chrome, not by color. If an action *truly* needs distinct affordance (e.g., destructive vs. confirming), use the accent for one and the neutral foreground for the other:

```css
.primary-action { background: #134E4A; color: #FAFAF8; }
.destructive-action { background: transparent; color: #0A0A0A; border: 1px solid #D6CDB8; }
```

**Rule violated:** color.md ("Exactly one accent color in the entire artifact").

---

## Forbidden: Bold body text by default

**Don't:**
```css
.summary { font-weight: 600; }
.headline { font-weight: 700; }
```

**Do (compliant):** Default to 400 / 500. Carry hierarchy with size, color, space, and tracking. If the user explicitly requests bold ("make the conclusion bold"), document the opt-in in a top-of-file comment:

```css
/* User opt-in: bold weights enabled for conclusion section (2026-05-15 request) */
.conclusion { font-weight: 600; }
```

**Rule violated:** `type-stack.md` Weight Usage ("Weight 600/700 reserved; requires explicit user opt-in").

---

## Forbidden: Serif typefaces

**Don't:**
```css
.pull-quote { font-family: 'Source Serif Pro', serif; font-style: italic; }
```

**Do (compliant):** Inter italic for emphasis, including pull quotes:

```css
.pull-quote {
  font-family: 'Inter', sans-serif;
  font-style: italic;
  font-weight: 400;
  font-size: 20px;
  color: #134E4A;
}
```

**Rule violated:** house-style.md Forbidden Patterns ("Serif typefaces anywhere").

---

## Forbidden: Emoji as visual elements

**Don't:**
```html
<h2>🚀 What's new</h2>
<li>✅ Faster builds</li>
<li>⚡ Better DX</li>
```

**Do (compliant):** Typography and structure carry the message. If you need visual markers, use SVG icons that match the type weight, or simply omit decoration:

```html
<h2>What's new</h2>
<li>Faster builds</li>
<li>Better DX</li>
```

**Rule violated:** house-style.md Forbidden Patterns ("Emoji glyphs as visual elements").

---

## Forbidden: Generic SaaS hero gradient

**Don't:**
```html
<section class="hero" style="background: linear-gradient(180deg, #5B5BE5, #1E1B4B);">
  <h1 style="color:white">Build the future</h1>
</section>
```

**Do (compliant):**
```html
<section class="hero" style="background: #FFFFFF;">
  <p class="label" style="font-size:10px;letter-spacing:0.12em;text-transform:uppercase;color:#6B7280">Q3 release</p>
  <h1 style="color:#0A0A0A;font-weight:500;letter-spacing:-0.025em">Build the future</h1>
</section>
```

**Rule violated:** Combination of color.md (no gradients, no purple) + `type-stack.md` (no default bold).

---

## Forbidden: Pastel pill chips

**Don't:**
```css
.tag-info { background: #DBEAFE; color: #1E40AF; }
.tag-warning { background: #FEF3C7; color: #92400E; }
.tag-success { background: #D1FAE5; color: #065F46; }
```

**Do (compliant):** Outlined chips in the accent, neutral, or foreground:

```css
.tag {
  background: transparent;
  border: 1px solid #D6CDB8;
  color: #0A0A0A;
  padding: 3px 8px;
  font-size: 10px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}
.tag-accent { border-color: #134E4A; color: #134E4A; }
```

**Rule violated:** color.md ("No pastel pill / chip backgrounds").

---

## Forbidden: Heavy shadows and overly-rounded components

**Don't:**
```css
.card {
  border-radius: 24px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.15), 0 4px 10px rgba(0,0,0,0.1);
}
```

**Do (compliant):**
```css
.card {
  border-radius: 8px;
  border: 1px solid #E5E7EB;
  box-shadow: 0 1px 2px rgba(0,0,0,0.04);
}
```

**Rule violated:** house-style.md Forbidden Patterns (border-radius > 12px, box-shadow beyond single subtle layer).

---

## Forbidden: Three-feature card grid with icons

**Don't:** Three identical cards in a row, each with an icon at the top, a heading, and a paragraph of body text. This is the universal "we are a SaaS product" template.

**Do (compliant):** If the content genuinely has three parallel points, present them as numbered editorial sections in a single column with type hierarchy carrying the structure. Reserve grids for *data*, not features.

```html
<section>
  <div class="num">01</div>
  <h3>First point</h3>
  <p>Body text explaining the point.</p>
</section>
<section>
  <div class="num">02</div>
  <h3>Second point</h3>
  <p>Body text explaining the point.</p>
</section>
```

**Rule violated:** house-style.md Forbidden Patterns ("Generic SaaS template tropes — three-feature-card-with-icon grids").

---

## Forbidden: Attention-seeking motion

**Don't:**
```css
.cta-button {
  animation: pulse 2s ease-in-out infinite;
}
@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}
```

**Do (compliant):** Motion serves comprehension. Hover and focus states are fine; on-load attention-seeking is not.

```css
.cta-button { transition: background-color 120ms ease-out; }
.cta-button:hover { background-color: #0E3D3A; }
```

**Rule violated:** motion.md Forbidden Motion Patterns ("Infinite loops", "Attention-seeking pulses").

---

## How to Add a New Anti-Pattern

When a new failure pattern is discovered:

1. Add a new section to this file with the same structure: forbidden example, compliant alternative, rule violated.
2. Add a corresponding line to critique-rubric.md if the failure isn't already covered.
3. Commit both changes together.