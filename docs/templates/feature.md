# Feature Authoring Guide

How to generate a feature document. A feature says what the product can do, for whom, and what that is worth. It does not say how the product is built.

**Legend**

- **Must** - required. Not doing it is a failure.
- **Should** - recommended. Follow it unless you have a reason not to, and say why if you don't.
- **Never** - prohibited.
- Unlabeled content is optional.

Read this file top-down. It is a guide, not a skill: nothing in it tells you how to conduct a conversation.

## How to use this guide

1. Read the [Glossary](#glossary) so the words mean here what they mean in a spec.
2. Read the [Rules for writing a feature](#rules-for-writing-a-feature).
3. Copy the [Template](#template).
4. Calibrate against the two [Examples](#examples). The minimal one shows the least that is still useful. The full one shows the same feature at full depth.
5. If a rule reads ambiguously, do not guess. See [Open items](#open-items).

The explore-workflow material this file used to carry - stance, question-asking, store selection, the capture procedure - now lives in `.opencode/skills/openspec-explore/SKILL.md` and its five sibling copies. It governs how an assistant behaves during a discovery conversation. It does not apply to authoring a feature.

## Glossary

| Term | Meaning |
|---|---|
| **Capability** | Something the product can do or enable. |
| **Feature** | A capability that provides value to one or more consumers. Describes what capability exists, not how it is built. May also modify an existing capability. |
| **Consumer** | A client, user, internal team, external system, or other stakeholder that benefits. The list is illustrative, not exhaustive. |
| **Value** | Client, business, operational, or other benefit, phrased in terms the consumer cares about. Name at least one concrete beneficiary. |
| **Behavior** | Something a consumer can do, or the system's response to it. One or the other, never conflated in a single line. |
| **Requirement** | A constraint or desired condition that still has to be translated into work. Not a task. |
| **Task** | A unit of implementation work. Not a requirement. |
| **Team** | The humans directing the work - the user and their collaborators. |
| **`[NEEDS CLARIFICATION: <path>]`** | An inline marker on a rule whose source is ambiguous. The bracketed path is the doc where the decision gets recorded. Fill the path in when that doc exists; delete the whole marker once the rule is settled. Find them all with `grep -rn 'NEEDS CLARIFICATION'`. |

**Feature boundaries** are the team's decision, based on the value the feature delivers and the scope of the work. They are not yours to infer from the code.

**Implementation dependencies** - what a feature needs from the rest of the system - are not part of the feature description. They are handled later, when the work is planned.

> **[NEEDS CLARIFICATION: docs/clarifications/____.md]**
> Where do implementation dependencies get recorded? The source says they are "handled later in the workflow" without naming the step. This document routes them to `design.md` (the decision) and `tasks.md` (the work).

**Keep the three apart.** A feature says what the product can do. A requirement says what must be true. A task says what gets built. A feature that has leaked into requirements and tasks is no longer implementation-independent.

> **[NEEDS CLARIFICATION: docs/clarifications/____.md]**
> Where is the line between a requirement and a task? A requirement states a condition that "still needs to be translated into work" - but the source does not say who performs that translation, at which step, or what a resulting task must contain.

## Rules for writing a feature

- **Must** describe the capability, not the build. A feature says what the product can do. The API, the schema, the data model, and the code belong in a design document.
- **Must** name a specific consumer. "Users" is not a consumer.
- **Must** state Value in the terms of a consumer named in the section above, and say why the value is needed rather than only what it is.
- **Must** write one consumer action per bullet under `### Consumer actions`, in the consumer's own words.
- **Must** list the system's responses in the same order as the actions they answer, one response per bullet.
- **Must** write an Outcome a reader could observe, not one they have to interpret.
- **Must** keep the headings exactly as the Template gives them. A document whose headings drift is not this template, and a reviewer cannot tell which parts you meant to change.
- **Should** put business rules, invariants, and vocabulary in `## Domain Context`, and keep implementation facts out of it.
- **Should** delete a section that genuinely does not apply rather than leaving it empty or half-filled.
- **Never** ship a document with unfilled placeholders. `<Name>` in the Template is an instruction to you, never text in an output.

## Template

````markdown
# Feature: <Name>

## Capability
What the product can do or enable. One or two sentences. No design decisions.

## Consumers
Who or what benefits. Name the specific consumer, not "users".

## Value
The benefit, and why it is needed. Tie it to a consumer named above.

## Behaviors

### Consumer actions
What a consumer can do, from their point of view, in their words. One action per bullet.

### System responses
What the system does in response to each action above. One response per bullet, in the same order.

## Outcome
What is true or possible once the feature exists. Observable, not aspirational.

## Domain Context
Business rules, invariants, terminology, and relationships to other capabilities a reader needs in order to understand the feature.
````

## Examples

Both examples are illustrative. Neither describes a capability cvx has, and neither is a cvx domain. They exist to show shape and depth. cvx has no product code yet, so the first real feature should replace both.

### Example 1 - minimal

An illustrative example, not a real capability. This is the minimum that is still useful.

```markdown
# Feature: Expense Claim Submission

## Capability
An employee can submit an expense for reimbursement, and finance can see it in a review queue.

## Consumers
An employee who has paid for a business expense. Finance staff who reimburse it.

## Value
Employees get reimbursed without emailing attachments. Finance gets one queue instead of scattered inboxes.

## Behaviors

### Consumer actions
- An employee submits a claim with an amount, a category, and a receipt.
- An employee checks the status of a claim they already submitted.

### System responses
- The system records the claim as `submitted` and places it in the review queue.
- The system shows the claim's current status and when it last changed.

## Outcome
A submitted expense claim is visible in a review queue with an amount, a category, and its status.

## Domain Context
A claim is an employee's request to be reimbursed for a business expense they already paid.
```

### Example 2 - full

An illustrative example, not a real capability. This is the same feature as Example 1, filled out.

```markdown
# Feature: Expense Claim Submission

## Capability
An employee can submit an expense for reimbursement. A finance reviewer can review the claim, request a missing receipt, approve it for payment, or reject it with a reason. Anyone involved can read the claim's history at every step, so nobody re-submits a claim to ask what happened to it.

## Consumers
- An **employee** who has paid for a business expense and is owed money back. Wants speed, not a form.
- A **finance reviewer** who approves or rejects claims and is accountable for the spend. Wants evidence, not promises.
- An **accounts payable clerk** who batches approved claims into a payment run. Wants a clean list, not a queue to re-triage.

## Value
- For the employee: reimbursement without chasing. A rejected claim says why, so the correction is a one-step fix rather than a guess.
- For finance: every claim arrives with the receipt attached and the policy checks already run, so review time is spent on judgment rather than on missing paperwork.
- For accounts payable: approved claims arrive in a state that can be paid in bulk, so the payment run does not re-check what a reviewer already decided.

This value is needed because reimbursement is a paper and inbox process with no shared state. Each consumer's work depends on work another consumer has not finished, and none of them can tell whose turn it is.

## Behaviors

### Consumer actions
- An employee submits a claim with an amount, a category, a date, and a receipt.
- An employee sees the status of a claim they already submitted.
- An employee attaches a missing receipt to a claim that finance returned for correction.
- A finance reviewer opens a claim from the review queue.
- A finance reviewer requests a receipt, giving a reason.
- A finance reviewer approves a claim.
- A finance reviewer rejects a claim, giving a reason.
- An accounts payable clerk pulls the approved claims for the current payment run.
- A finance reviewer reads a claim's full history.

### System responses
- The system records the claim as `submitted`, runs the receipt and age checks, and places it in the review queue. A claim failing either check is recorded as `needs-receipt` and is not queued for review.
- The system shows the claim's current status, when it last changed, and what is outstanding.
- The system appends the receipt to the claim, returns it to `submitted`, and re-runs the checks.
- The system shows the claim in full, including its receipt, its checks, and its history.
- The system sets the claim to `needs-receipt`, records the reviewer's reason, and notifies the employee.
- The system sets the claim to `approved` and makes it eligible for the next payment run.
- The system sets the claim to `rejected` with the reason attached. A rejected claim is final; correcting it means a new claim.
- The system returns the approved, unreimbursed claims, oldest first, as a single list.
- The system returns the claim's history in order, newest first, naming who did what and when.

## Outcome
An employee can submit a business expense and know, without asking anyone, what state it is in and what is needed to move it forward. A finance reviewer can decide a claim from the claim page alone. An accounts payable clerk can produce a payment list that needs no re-checking. Every state change is attributable to a person and a time.

## Domain Context
- **Claim** - an employee's request to be reimbursed for a business expense they have already paid. A claim is not an invoice, and a claim is never settled twice.
- **Receipt requirement** - a receipt is mandatory for any claim at or above **25.00** in the claim's currency. Below that a receipt is optional, but the employee may still attach one.
- **Submission window** - a claim must be submitted within **30 days** of the expense date. Past 30 days the system records it as `expired` and does not queue it. A missing receipt does not pause the window.
- **Currency rounding** - a foreign-currency claim is converted at the rate on the expense date, then rounded **down** to the nearest whole unit. Rounding down can never create a shortfall for the employee, and the applied rate is stored on the claim so the figure cannot change later.
- **Payment runs** - a payment run takes a snapshot of the approved claims when it starts. A claim approved after that point waits for the next run. This is why approval and payment are separate states.
- **Terminology** - "claim" is the employee's request throughout. "Expense" is the thing being claimed for. They are not interchangeable, and a document using both for one object is wrong.
```

### What grew between the two examples

Both examples describe the same feature, so every difference below is a difference in depth, not in subject.

- **Capability** - one sentence becomes four. The minimal version says what happens; the full version adds the reviewer's three possible decisions and the history, because a reader who cannot see those states cannot tell whether the capability is complete.
- **Consumers** - two named parties become three. The minimal version quietly assumed the reviewer and the payer were the same person, which is the kind of assumption that quietly becomes a design error.
- **Value** - one sentence per consumer becomes a value *and* a reason, plus the shared problem that makes the feature worth building at all.
- **Behaviors** - two actions become nine, split across three consumers. Each consumer gets their own actions, so nobody has to infer that "review" means approve, reject, *and* request a receipt.
- **Outcome** - one observable sentence becomes four, one per consumer, each naming who can now do what without asking a person.
- **Domain Context** - one definitional sentence becomes six entries. The minimal version defines the object. The full version adds the rules a reader would otherwise have to guess: the threshold, the deadline, the rounding, and why payment is a separate state.

## Open items

Two rules in this guide are still ambiguous. Each is marked in place with a clarification marker - see the [Glossary](#glossary) - and the marker carries a path placeholder until the decision is recorded.

- **Where implementation dependencies get recorded.** The source says they are handled later, without naming the step. This guide routes them to `design.md` for the decision and `tasks.md` for the work.
- **Where the line between a requirement and a task falls.** A requirement states a condition that still has to be translated into work, but nothing here says who does that translation or at which step.

Resolve both before the first real feature, not after. Every feature inherits the answers.
