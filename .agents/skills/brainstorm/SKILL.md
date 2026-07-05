---
name: brainstorm
description: Turn a vague idea into a right-sized spec through a short design dialogue BEFORE any code. INVOKE when the user says "brainstorm", "help me think through X", "what should we build", presents a fuzzy/ambitious request, or seems unsure of scope. Ends with an approved spec doc ready for /commonkit:implement.
disable-model-invocation: true
user-invocable: true
---

# Brainstorm

Design first, code never — until a human signs off. Most wasted work comes from
building the wrong thing confidently. This skill front-loads the disagreement,
with a sharp YAGNI bias: the first real question is always *should this exist at
all*.

## Hard rule

**No implementation actions until the spec is presented AND approved.** No files
written, no scaffolding, no "I'll just start". This holds even for "trivial"
tasks — the trivial ones hide the unexamined assumption that costs you a day.
For a genuinely small ask the spec can be five lines, but it still gets shown
and approved.

## Flow

1. **Ground it.** Read the relevant code/docs before asking anything. Come in
   with context, not a blank stare.
2. **Interrogate the premise.** First question is whether the thing needs to
   exist — is there a stdlib/native/existing-code path that removes the work?
   If yes, say so and stop.
3. **One question at a time.** Ask the single highest-leverage clarifying
   question, wait for the answer, then decide the next one from it. Never dump a
   questionnaire. Stop when you can describe the solution without guessing.
4. **Propose 1–3 approaches.** For each: what it is, the main tradeoff, and why
   you'd pick it. State your recommendation plainly.
5. **Present the spec.** Problem, chosen approach, scope boundaries (explicitly
   what is *out*), key decisions, and open risks. Right-sized to the task.
6. **Loop until sign-off.** Revise on feedback. Do not proceed on silence.
7. **Save + hand off.** Write the spec to `docs/specs/<slug>.md` and tell the
   user it's ready for `/commonkit:implement`.

## Notes

- Offer a quick diagram or mock only when it genuinely clarifies a layout or
  flow — don't manufacture ceremony.
- Keep your own prose short. The dialogue is the product, not your summaries.
