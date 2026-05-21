# Format Decision Tree

Used during the format consultation step (Behavioral Rule 1). The agent asks up to 3 targeted questions, then uses the answers to recommend a medium and route to a downstream skill.

---

## The Four Diagnostic Axes

| Axis | Options |
|---|---|
| **Goal** | inform / persuade / decide / instruct / explore |
| **Audience** | peer / report / executive / external / public |
| **Distribution** | shared file / printed / screen-walked-through / live-presented / interactive |
| **Density** | brief moment / 2–5 min read / 10–20 min read / extended |

Ask the smallest number of questions needed (1–3). Skip axes when the user has already named them. Skip the entire consultation when the user has named the medium explicitly ("draft a board memo").

---

## Decision Mappings

### Goal = decide AND audience = executive

| Distribution | Density | → Medium | → Skill | → Layout |
|---|---|---|---|---|
| shared file | brief moment | **One-pager** | `format-document` | `one-pager` |
| printed | 2–5 min | **Board memo** | `format-document` | `board-memo` |
| live-presented | brief moment | **Title + decision deck** | `frontend-slides` | (deck) |

### Goal = inform AND audience = executive / external

| Distribution | Density | → Medium | → Skill | → Layout |
|---|---|---|---|---|
| shared file | 2–5 min | **Executive brief** (deferred) → falls back to **board memo** | `format-document` | `board-memo` |
| shared file | 10–20 min | **Long-form report** (deferred) → falls back to **board memo** | `format-document` | `board-memo` |
| printed | any | **Board memo** | `format-document` | `board-memo` |

### Goal = persuade

| Distribution | Audience | → Medium | → Skill |
|---|---|---|---|
| live-presented | any | **Pitch deck** | `frontend-slides` |
| screen-walked-through | any | **Walkthrough deck** | `frontend-slides` |
| shared file | external | **One-pager** | `format-document` |

### Goal = instruct

| Distribution | Density | → Medium | → Skill |
|---|---|---|---|
| screen-walked-through | 2–5 min | **Walkthrough deck** | `frontend-slides` |
| shared file | extended | **Long-form report** (deferred) → use board-memo for now | `format-document` |
| interactive | any | **Interactive page** | `superpowers:frontend-design` |

### Goal = explore (open-ended, interactive)

| → Medium | → Skill |
|---|---|
| **Interactive page / explorer** | `superpowers:frontend-design` |
| **GUI / web app** | `superpowers:frontend-design` |

---

## Deferred Layouts

The following are NOT yet implemented in `format-document` (per scope discipline — only `board-memo` and `one-pager` shipped initially):

- Executive brief
- Long-form report (with TOC, figures, page numbers)
- Formal letter (heading block + signature)
- Informal memo

When consultation maps to a deferred layout, fall back to `board-memo` (most universal) and surface the limitation: "The ideal layout for this would be the executive brief, which isn't built yet. Falling back to the board memo, which handles this content acceptably. Want me to flag this as a layout to add?"

---

## Surfacing the Routing Decision

After consultation, always surface the routing in the form:

> "Routing to `<skill>` with the `<layout>` layout. (Reasoning: <one-sentence justification tied to the diagnostic answers>.)"

Example:

> "Routing to `format-document` with the `board-memo` layout. (Reasoning: goal=decide + audience=executive + density=2–5 min + distribution=shared file → memo, not deck.)"

---

## Edge Cases

- **User has already named the medium.** Skip consultation entirely. Confirm understanding ("Drafting a board memo, noted") and proceed to drafting.
- **User answers all four axes in their first message.** Skip consultation; route immediately.
- **Answers don't fit any mapping row.** Pick the closest fit, surface it, and ask one clarifying question.
- **User requests a deferred layout explicitly.** Fall back as above, but also note in the response: "Logged as a layout to add to format-document/references/layouts.md."
