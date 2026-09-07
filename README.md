# house-designer

Claude agent — design director persona for cross-medium aesthetic enforcement. Engaged for documents, slides, walkthrough presentations, and GUI work. Owns the locked house style (Inter + Teal 900 + warm/stark surfaces + no serif + no default bold) and enforces it via critique-then-revise.

## Structure

| Path | Purpose |
|---|---|
| `agent.md` | The agent definition. This is the file Claude loads when engaging the persona. |
| `references/` | Critique rubric, anti-patterns, format decision tree. The agent reads these mid-task. |
| `knowledge/` | The locked house style — color, type stack, density, motion, examples. Bundled snapshot of `ai-lab/knowledge/house-style/`. |

## Engagement

From any Claude session that has this agent available:

> "Engage the house-designer agent for [task]."

The agent will run format consultation, produce a previewable artifact, and critique-revise until the rubric passes.

## Installation (other machines)

```bash
git clone https://github.com/amaranta-ays/house-designer.git ~/ai-lab/agents/house-designer
```

Or pin to a tagged version:

```bash
git clone --branch v0.2.0 https://github.com/amaranta-ays/house-designer.git ~/ai-lab/agents/house-designer
```

## Release flow

1. Edit the source of truth — usually `~/ai-lab/knowledge/house-style/*` for the design rules, or `agent.md` / `references/*` for the persona logic.
2. If you edited the house-style knowledge, run `~/ai-lab/scripts/sync-house-style.ps1` to refresh the bundled `knowledge/` snapshot in this repo.
3. Commit + push.
4. `git tag vX.Y.Z && git push origin vX.Y.Z`.
5. Consumers re-clone or `git pull` then `git checkout vX.Y.Z`.

## Notes

`knowledge/` is a bundled copy of the design rules originally maintained in `ai-lab/knowledge/house-style/`. When the source-of-truth rules evolve in AI-Lab, copy into this repo's `knowledge/` and cut a new tag. Cross-language renderers (e.g., `house-style-render-ts`) bundle parallel copies — keep them in sync at release time.

Sister repo: [house-style-render-ts](https://github.com/amaranta-ays/house-style-render-ts) — the runtime renderer that applies these rules to markdown.
