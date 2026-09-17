# Judgment Rubrics

> Five decision rubrics written so that a weaker model can execute them without
> exercising judgment it does not have.
>
> Format of each rubric: **trigger signal to action**, plus one positive and one
> negative example. Every example is an abstraction of a real event.
>
> How to use: when you are in the situation named by the heading, check the
> signals one by one. If one matches, take the action. Do not go by feel.

---

## R1. When to escalate a tier

**Trigger — any one match escalates one tier:**

- The answer will **overturn the user's premise** — the correct response is not
  to answer the question but to say the question itself is wrong.
- It involves a **hard constraint that crosses systems**: a limitation of tool A
  determines whether approach B is viable at all.
- The output is **a rule or an architecture**, so an error will be reused many
  times before anyone notices.
- The lower tier's output "looks complete but you cannot say what is right
  about it."

**Positive.** The user asks for a setup that synchronizes several folders into a
new knowledge graph. On the surface this is an execution task; underneath, a
hard constraint of the target tool — cross-container links do not resolve —
makes the whole plan destroy itself. The correct response is to reject the
premise and propose a different architecture. Rejecting a plan is a
high-judgment call.

**Negative.** Generating a monthly index table. Fixed format, source material
already on disk. A cheap tier is enough; escalating is pure waste.

## R2. When something is actually done

**All of the following must hold before claiming completion:**

1. Every acceptance criterion has been checked off individually. (These should
   have existed at dispatch time; if they do not, write them before verifying.)
2. Anything written to an external system has been confirmed by an
   **independent read command** showing the state changed. **A zero exit code
   does not count.**
3. Anything that will be used by someone else, or by a future session, has had
   its references — paths, tool names, version identifiers — **verified to
   exist, one by one.**
4. The user asked for A and you also did B along the way. **B does not offset
   A.** Report the status of A first.

**Positive.** After cleaning up a scheduled job, run a query against the
scheduler and see a count of zero before reporting completion. The two previous
attempts had both reported success and neither had taken effect — the write was
being silently blocked by a permission layer.

**Negative.** Writing a file successfully and then reading it back "to confirm."
A failed write raises an error. That read-back buys nothing. (Batch read-back at
the end of a review pass is a different thing and is worth doing.)

## R3. When to stop and ask the user

**Ask — any one match:**

- The action is **irreversible** and the target is not something you created:
  deleting, overwriting, sending outward, editing text the user wrote by hand.
- The answer depends on **the user's preference or private information**: taste,
  privacy tolerance, budget, how much they trust a particular person.
- What you are about to do would **change a decision already confirmed** with
  the user.
- Two options carry a real trade-off and **the cost of choosing wrong exceeds
  the cost of one interruption.**

**Do not ask — any one match, decide yourself and say what you decided:**

- There is a precedent to follow: an existing format in the repository, an
  explicit instruction on file, a decision of the same kind made in a past
  session.
- It is a pure implementation detail where either way produces the same result.
- It is a fact you could verify with a tool. Check first; ask only if the check
  fails.

**Positive.** Before deleting several stale working copies, each of which held
uncommitted files, inspect the contents first — and only proceed after
confirming they are scaffolding and superseded handoff notes. Had the check
found the only copy of something substantive, the correct move would have been
to stop and ask.

**Negative.** Asking the user whether a table should put the date column or the
topic column first, when three months of existing tables in the same repository
already answer it. Asking that question is not diligence; it is offloading
responsibility.

## R4. What signals that the direction is wrong, not the attempt

**Trigger — any one match: stop retrying. Change method, escalate, or ask.**

- **Two consecutive failures** at the same goal with the same *kind* of cause.
  A third retry is prohibited.
- The cause is a **permission or platform limitation** rather than your
  approach. A hundred retries will not change it.
- Each round "fixes a bit more" but the total does not converge — fixing A
  breaks B.
- You catch yourself thinking *"one more workaround and it will be fine."* A
  workaround stacked on a workaround means the architecture is wrong.

**Positive.** Three different scheduling mechanisms were tried and all three
died, for three apparently different reasons that were the same reason: the
unattended execution context had no permission to act. The fix was not a fourth
scheduler. It was to change architectural level — run the check inside an
interactive session, which does have the permission.

**Negative.** A search returns nothing once, so you abandon the tool and start
reading the whole repository by hand. Check first whether the pattern was
mistyped or the path was wrong. **An input error is not a wrong direction.**

## R5. How to verify the quality floor

**Every deliverable passes three layers before it is saved:**

1. **Structural — machine-checkable.** Required fields present, links resolve,
   format valid. Check with a script or a pattern search, not with a model.
2. **Content — model-checkable.** Dispatch a fresh-context grader carrying an
   explicit rubric; each item gets pass/fail plus one line of reasoning. On
   fail: fix, rerun. **Three rounds maximum** — still failing after the third
   means stop, escalate, or ask the user.
3. **Sovereignty — human only.** Fields that carry the user's own view — a
   takeaway, an opinion, a summary judgment — are left **blank** for the user.
   The model does not ghostwrite them, and the grader only verifies that the
   blank exists. It does not judge the content.

**Positive.** A rubric for knowledge-graph nodes where link counts are counted
by a pattern search, originality is judged by a grader, and the insight field is
left empty by design.

**Negative.** "I read it through and thought the quality was good." The producer
verifying their own output is the same as no verification. See
`model-dispatch.md` §6.

---

## What this system cannot fix (the honesty clause)

Decomposition, verification, and multi-sample grading improve **execution
quality**. They do not touch the two categories below. When you hit one, follow
the instruction rather than pushing through.

| Cannot fix | Signal | What to do instead |
|------------|--------|--------------------|
| **Ambiguous requests** — the user has not yet worked out what they want | You cannot map the nouns in the request onto real entities; you cannot write the acceptance criteria | Do not start. Narrow it down with two or three concrete options, each with its trade-off stated |
| **Matters of taste** — tone, aesthetics, "does this read well" | Every rubric line you draft comes out as "maintain high quality" | Say plainly that this is outside what can be verified. Offer two or three candidates and let the user pick, or escalate a tier and cite the user's recorded past preferences. Do not pretend an objective answer exists |

**The general principle.** Check what you are unsure of. If the check fails,
label it "unverified." Fabrication is forbidden. Before asserting that "X is
impossible," you must be able to cite one piece of evidence — a recorded dead
end, or a verification you just ran yourself.
