# /grilling

Interview me relentlessly about every aspect of this spec/plan until we reach a shared
understanding. Walk down each branch of the design tree, resolving dependencies between
decisions one-by-one. For each question, provide your **recommended** answer.

Ask the questions **one at a time**, waiting for feedback on each before continuing. Asking
multiple questions at once is bewildering.

If a question can be answered by exploring the codebase, **explore the codebase instead** of asking.

> Adapted from Matt Pocock's `grilling` skill — https://github.com/mattpocock/skills

## When used as the SDD "clarify" step

This is `specs/README.md` step 2 and step 4 of `/new-spec`. When grilling a draft spec:

1. **Source the questions** from the spec's "Open questions (clarify)" list, plus any acceptance
   criterion that isn't yet **testable** (one you can't write a failing test for is an open question).
2. **Grill** one question at a time, each with your recommended answer.
3. **Record** each resolved answer back into `spec.md` and remove it from the open-questions list —
   the spec, not the chat, is the source of truth (constitution §4).
4. **Flip the status** `draft → clarified` only when that list is empty and every AC is testable.

Don't start `plan.md` until the spec is `clarified`. You can also point this at a finished `plan.md`
to stress-test the *how* before implementing.
