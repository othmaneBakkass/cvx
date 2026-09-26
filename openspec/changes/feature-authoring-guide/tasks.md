# Tasks

Single file changes: `docs/templates/feature.md`. Groups are ordered by dependency — the guide's canonical heading list is established in group 4 and every later group verifies against it.

## 1. Baseline

- [x] 1.1 Commit the current 434-line state of `docs/templates/feature.md` so the Migration Plan rollback (`git checkout -- docs/templates/feature.md`) has a target, or confirm with the user that an uncommitted baseline is acceptable. Verify: `git status --short docs/templates/feature.md` shows a clean entry, or the user has explicitly declined to commit.

## 2. Header and orientation

- [x] 2.1 Replace the title and purpose paragraph with a task-oriented opening that names the file's job as generating a feature document, and point at `.opencode/skills/openspec-explore/SKILL.md` as the home of the explore-workflow material being removed. Verify: the file's first heading reads `# Feature Authoring Guide`, and `grep -c 'openspec-explore/SKILL.md' docs/templates/feature.md` returns exactly 1.
- [x] 2.2 Keep the `Must` / `Should` / `Never` legend unchanged and add one line stating that the file is read top-down. Verify: `grep -o '\*\*Must\*\*\|\*\*Should\*\*\|\*\*Never\*\*' docs/templates/feature.md | sort -u` returns all three labels.

## 3. Glossary and authoring rules

- [x] 3.1 Move the Definitions table into the glossary position, changing only the final row: replace the `Needs clarification` row with the `` **`[NEEDS CLARIFICATION: <path>]`** `` row. Verify: the table has 9 term rows and `grep -c 'NEEDS CLARIFICATION' docs/templates/feature.md` returns 4 — the two callouts from 3.3 plus the two occurrences inside the legend row.
- [x] 3.2 Add a "Rules for writing a feature" section carrying the authoring rules already implied by the retained material: a feature is implementation-independent; one consumer action per bullet; system responses listed in the same order as the actions; Outcome is observable, not aspirational; no section ships with unfilled placeholders; headings must match the template exactly. Verify: every rule carries a `Must`, `Should`, or `Never` label, and the "headings must match the template exactly" rule is present.
- [x] 3.3 Place the two retained `[NEEDS CLARIFICATION: docs/clarifications/____.md]` callouts directly after the rules they qualify, preserving their existing text and their `____` path placeholders verbatim. Verify: `grep -c 'NEEDS CLARIFICATION' docs/templates/feature.md` returns 4, and `grep -c 'docs/clarifications/____.md' docs/templates/feature.md` returns 2.

## 4. Template skeleton

- [x] 4.1 Move the Feature Template block into its position after the rules, keeping its 4-backtick fence. The skeleton must contain exactly these headings, in this order: `# Feature: <Name>`, `## Capability`, `## Consumers`, `## Value`, `## Behaviors`, `### Consumer actions`, `### System responses`, `## Outcome`, `## Domain Context`. Verify: `grep -n '^#\{1,3\} ' docs/templates/feature.md` shows all nine, and the fence markers are balanced (`grep -c '^````' docs/templates/feature.md` returns 2).

## 5. Minimal example

- [x] 5.1 Add a minimal worked example below the skeleton, on expense reimbursement claims, carrying a visible "illustrative example" label and a note that it demonstrates minimum sufficient detail. Verify: the label appears once in the example block, and the block is under 30 lines.
- [x] 5.2 Verify heading fidelity against 4.1 — the minimal example uses the identical nine headings in the identical order, with only `# Feature: <Name>` replaced by a real name. Verify: extracting the example's `^#\{1,3\} ` lines and comparing them to 4.1's list yields no differences.

## 6. Full example

- [x] 6.1 Add a full worked example on the same feature, carrying its own "illustrative example" label, with more consumer actions and system responses, consumers whose positions differ, and a Domain Context carrying the receipt threshold, submission window, and currency-rounding rules. Verify: the three domain rules are each present in the block.
- [x] 6.2 Verify heading fidelity against 4.1 for the full example too, with `# Feature: <Name>` replaced by a real name. Verify: extracting the example's `^#\{1,3\} ` lines and comparing them to 4.1's list yields no differences.
- [x] 6.3 Add a short contrast note after the full example naming which sections grew relative to the minimal example and why, so the depth calibration is explicit rather than left to inference. Verify: the note names Capability, Behaviors, and Domain Context.

## 7. Remove the explore-workflow body

- [x] 7.1 Delete the eight sections carried over from `openspec-explore/SKILL.md` — Hard Rules, Confirmation, How to Explore a Feature, Store and Project Setup, Discover Existing Context, Capture Insights, Ending a Session, and Optional — and confirm no remnant references a store, a change directory, or a capture procedure. Verify: `grep -ci 'openspec store\|opsx-explore\|openspec new change\|--store' docs/templates/feature.md` returns 0.

## 8. Integration checks

- [x] 8.1 Run the full verification set from the design's Migration Plan: section list, `grep -c 'NEEDS CLARIFICATION'` returning 4, balanced fence markers, and `grep -nP '[^\x00-\x7F]'` returning nothing. Verify: all four checks pass in one pass, with the commands and their output recorded in the apply summary.
- [x] 8.2 Confirm the retained material survived the rewrite unchanged — the Definitions table rows and the template skeleton must be context or `+` lines in the diff, never `-` lines. Verify: `git diff docs/templates/feature.md | grep '^-'` contains no line from the Definitions table or the template skeleton.
- [x] 8.3 Record the final line count against the design's 200-230 line estimate. Verify: `wc -l docs/templates/feature.md` falls in range, or the deviation is reported in the apply summary with its cause.
