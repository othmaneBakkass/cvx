# Design

## Context

`docs/templates/feature.md` is 434 lines, 21,031 bytes, and currently modified but uncommitted. The only project-owned content in the repository is this file and an empty `docs/README.md`; `git ls-files` shows OpenSpec scaffolding across six agent directories plus `mise.toml`. There is no product code, no test suite, and no stated domain.

Measured layout of the current file:

| Lines | Section | Fate |
|---|---|---|
| 1-15 | Title, purpose, `Must`/`Should`/`Never` legend | keep |
| 16-36 | Hard Rules | delete (see D5) |
| 37-64 | Confirmation | delete |
| 65-94 | Definitions | keep |
| 95-129 | Feature Template | keep |
| 130-210 | How to Explore a Feature | delete |
| 211-258 | Store and Project Setup | delete |
| 259-319 | Discover Existing Context | delete |
| 320-397 | Capture Insights as a Change | delete |
| 398-424 | Ending a Session | delete |
| 425-434 | Optional | delete |

The 354 deleted lines are the body of `openspec-explore/SKILL.md`. Verified identical: the same sections, same rules, same store-selection and project-check procedures. That file is installed at `.opencode/skills/openspec-explore/SKILL.md` and in five other agent directories, so nothing is lost.

The file also carries two `[NEEDS CLARIFICATION: docs/clarifications/____.md]` markers, on implementation-dependency routing (line 85) and the requirement-to-task boundary (line 90). The user has deferred both.

See `proposal.md` for motivation.

## Goals / Non-Goals

**Goals**

- An AI opening this file can produce a correctly-shaped feature document without inference.
- Every template section is demonstrated at two calibrations — minimum sufficient, and thorough.
- A reader can tell which sections of the current file were kept and why, from the file itself.
- The guide is self-contained: no section requires opening another file to be understood.

**Non-Goals**

- Specifying cvx's domain. The repository has none, and inventing one in a process document would put fabricated product intent into a file every future feature inherits.
- Replacing `openspec-explore/SKILL.md` or editing any of the six copies.
- Resolving the two deferred `[NEEDS CLARIFICATION]` markers.
- Adding a `docs/README.md` index, or any link to this file (see Open Questions).
- Any test, lint, or CI change. There is nothing to run markdown against.

## Decisions

### D1 — One file, not a guide plus a separate examples file

Keep everything in `docs/templates/feature.md`.

*Rationale:* the stated problem is that an AI opening the file gets no worked example. Splitting examples into `docs/templates/feature-example.md` means the AI must discover and open a second file — reproducing the original failure with one more indirection.

*Alternative considered:* `feature.md` (rules) + `features/example-expense-claim.md` (example). Rejected for the reason above.

### D2 — Task-oriented order: rules before rationale, examples last

```
Title + purpose + legend
How to use this guide
Glossary (Definitions)
Rules for writing a feature
Feature Template (bare skeleton)
Example 1 - minimal
Example 2 - full
Clarification markers and open items
```

*Rationale:* the file's job is to be used top-down by something that has not read it before. A reader who needs an example has to survive the rules first; a reader who needs the rules should not have to read an example first. Examples last also keeps the two new blocks adjacent, so the contrast between them is immediate.

*Alternative considered:* preserve the current 1-10 numbering. Rejected — the numbering encodes the old skill-shaped order, and leaving gaps in it would be worse than renumbering.

### D3 — Both examples use the same domain, escalating in depth

Both examples describe **expense reimbursement claims**. The minimal example is the same feature at minimum sufficient detail; the full example is the same feature filled out.

*Rationale:* the skill being taught is *calibrating depth per section*, not learning a subject. Holding the subject fixed isolates the variable — a reader can see exactly which sections grew, and why that growth is the difference between a thin document and a usable one. Two different domains would add a second variable, and a reader could no longer tell whether a short section reflects a simple feature or a change of topic.

*Alternative considered:* two unrelated domains to also vary the subject. Rejected as above.

### D4 — Illustrative domain, explicitly labeled

Expense claims is invented for this guide, chosen because it is mundane enough that a reader attends to structure rather than content, and rich enough to exercise every section: consumers with competing interests (submitter wants speed, finance wants evidence), behaviors with visible system responses (submit → validated or rejected with reasons), observable outcomes, and real domain rules (receipt threshold, submission window, currency rounding) that demonstrate what belongs in Domain Context.

*Alternative considered:* a `cvx`-plausible domain such as convex optimization. Rejected — the repository name is the only evidence, and a process document that asserts a product domain it cannot support is worse than one that admits it is illustrative.

Both examples carry a visible "illustrative example" label. A reader must never mistake them for existing cvx capabilities.

### D5 — Hard Rules deleted; authoring rules kept

The user asked to keep "the definition and rules and glossary." Applied to the file, that is:

- **Keep** the `Must`/`Should`/`Never` legend, the Definitions table, the template, the marker convention, and a new short *Rules for writing a feature* section drawn from rules that already appear in the retained material.
- **Delete** the Hard Rules block. Its contents — never implement, never write without confirmation, never hand-create a change directory, never fake understanding — are instructions for an agent *inside an explore conversation*. An agent using this guide is authoring a feature, not exploring, so those rules would not apply to it. They remain in `openspec-explore/SKILL.md`, where they are in force.

*Alternative considered:* retain Hard Rules under an appendix. Rejected — it reintroduces the skill shape the change is removing, and the content is preserved in six other files.

*This is the one judgment call in the change and it is reversible:* restoring the block is a copy-paste from the current file or from the `SKILL.md`.

### D6 — Three copies of the headings, by contract

The bare skeleton and both examples carry identical heading text, so heading fidelity is demonstrated three times. This is deliberate duplication.

*Risk and mitigation:* the three can drift. Mitigation is an explicit self-check rule in the guide ("the headings in your document must match the template exactly") plus a `grep` verification in `tasks.md` that compares heading lines across all three.

*Alternative considered:* drop the bare skeleton once examples exist. Rejected — the skeleton is what an author copies; the examples are what an author calibrates against. Both jobs are real.

## Risks / Trade-offs

- **Deleted content is missed by a reader who expected it** → All 354 lines are preserved verbatim in `.opencode/skills/openspec-explore/SKILL.md` and five sibling copies. Rollback is one command (see Migration Plan). The guide's *How to use this guide* section will name the `SKILL.md` as the home of the explore-workflow material, so the trail is explicit rather than a dead end.

- **Three heading sets drift apart over time** → Self-check rule plus `grep` comparison in `tasks.md`. Accepted cost of D6.

- **The examples calcify as cvx's actual domain** → Both are labeled illustrative, and D4's rationale is recorded here and in the guide. The first real feature should replace them. Recorded as a known follow-up rather than solved now, because no feature exists yet.

- **"Rules" read too narrowly, losing the Hard Rules block** → D5 is stated explicitly above and flagged in the final summary for redirect. Cheap to reverse.

- **Length estimate in the proposal is wrong** → `proposal.md` predicts 150-180 lines. It undercounts, because it budgeted the retained 65 lines plus a skeleton and did not budget example content. The realistic figure is **200-230 lines**, still a reduction from 434. The plan is unaffected; only the number in the proposal is stale.

- **Guide is never found** → Nothing in the repository links to `feature.md`, and this change does not add a link. Tracked as an Open Question rather than silently expanded into scope.

## Migration Plan

Single-file documentation edit, no deploy, no runtime effect.

1. Rewrite `docs/templates/feature.md` in place. One commit.
2. Verify: `grep -n '^#\{1,3\} '` shows the new section list; `grep -c 'NEEDS CLARIFICATION'` returns 4, matching the two markers plus the two occurrences in the legend row; fence markers are balanced; the file is pure ASCII.
3. Rollback: `git checkout -- docs/templates/feature.md`. Note the file is currently modified-but-uncommitted, so commit before editing if the pre-change state matters.

No ordered rollout, no feature flag, no backfill.

## Open Questions

- Should `docs/README.md` gain a link to the guide? Deferrable — it changes neither the approach nor the task breakdown, and nothing references the file today. Answer when cvx gains its first real feature and the docs tree stops being empty.
