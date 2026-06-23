---
name: grilling
description: Interview the user relentlessly, one question at a time, to remove ambiguity from a spec or a plan before building. This is the engine of the SDD "clarify" step (specs/README.md) — use it to grill a draft spec's open questions, or any time the user wants to stress-test a plan/design before code. Triggers on "grill me", "clarify", "stress-test this".
---

Interview me relentlessly about every aspect of this spec/plan until we reach a shared
understanding. Walk down each branch of the design tree, resolving dependencies between
decisions one-by-one. For each question, provide your **recommended** answer.

Ask the questions **one at a time**, waiting for feedback on each before continuing.
Asking multiple questions at once is bewildering.

If a question can be answered by exploring the codebase, **explore the codebase instead** of asking.

> Adapted from Matt Pocock's `grilling` skill — https://github.com/mattpocock/skills

## When used as the SDD "clarify" step

This is `specs/README.md` step 2 and step 4 of the `new-spec` skill. When you're grilling a
draft spec:

1. **Source the questions** from the spec's "Open questions (clarify)" list, plus anything in
   the acceptance criteria that isn't yet **testable** (an AC you can't write a failing test
   for is an unresolved question).
2. **Grill** one question at a time as above, each with your recommended answer.
3. **Record** each resolved answer back into `spec.md` — fold it into the relevant section and
   remove it from the open-questions list. The spec, not this chat, is the source of truth (§4).
4. **Flip the status** `draft → clarified` only when that list is empty and every AC is testable.

Do not start `plan.md` until the spec is `clarified`. (You can also point this skill at a
finished `plan.md` to stress-test the *how* before implementing.)
