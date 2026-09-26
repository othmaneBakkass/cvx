# Proposal

## Why

`docs/templates/feature.md` is currently shaped like an agent skill, not like a guide for generating features. Its ten sections are mostly instructions for *how an assistant should behave during an explore conversation* — stance, question-asking, store selection, capture procedure — and roughly 350 of its 434 lines duplicate the body of `openspec-explore/SKILL.md`, which is already installed in six agent directories.

An AI asked to generate a feature from that file gets no worked example. The one thing that would teach it the shape is a skeleton with `<bracketed placeholders>`, which does not show how much detail each section needs, what a good Outcome sounds like versus a vague one, or what belongs in Domain Context and what does not.

## What Changes

- Restructure `docs/templates/feature.md` from a skill-shaped document into a task-oriented guide for authoring a feature: glossary first, then the rules, then the template, then worked examples.
- Add **two worked examples** — one minimal example that shows the shape, one full example that shows depth. Both are explicitly labeled as illustrative.
- Keep the `Must` / `Should` / `Never` legend, the definitions table, the feature template, and the `[NEEDS CLARIFICATION: <path>]` marker convention, including its two existing open items.
- Remove the explore-workflow body: Confirmation, How to Explore a Feature, Store and Project Setup, Discover Existing Context, Capture Insights, Ending a Session, Optional, and the Hard Rules block. All of it is agent-conduct guidance that already lives in `openspec-explore/SKILL.md`.
- **Not a breaking change** for any consumer: the only consumer of this file is a human or AI reading it, and the retained sections keep their current meaning.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

None.

This change is documentation only. It restructures a markdown guide, adds examples, and deletes duplicated prose. No code, API, CLI surface, or observable system behavior changes, and the project has no product code to begin with — `git ls-files` contains only OpenSpec scaffolding, `mise.toml`, and `docs/`. Per the spec-driven rule that specs describe behavior and a docs-only change must not invent a requirement to satisfy validation, this change opts out of spec deltas via `skip_specs: true` in `.openspec.yaml`.

The rules and vocabulary that the guide enforces are not spec-level behavior; they are content *of the artifact being edited*, so they are specified in the guide itself rather than in `openspec/specs/`.

## Impact

**Affected files**

- `docs/templates/feature.md` — restructured, roughly 434 lines to roughly 150-180.

**Affected code / APIs / dependencies**

None. The change touches no source file, no dependency, and no command. `docs/README.md` remains empty and unreferenced; nothing in the repository links to `feature.md` today, so no other file needs updating.

**Consumers**

- AI agents asked to author, modify, or review a feature document. They gain two worked examples to pattern-match against, and lose the explore-workflow material they were never meant to consume.
- Human reviewers of feature documents. They gain the same examples; the glossary and template they already relied on are unchanged.
