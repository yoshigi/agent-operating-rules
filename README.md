# Agent Operating Rules

> Rules for a *person* working with AI agents — not an SDK, not an execution
> framework. (Vendors use "harness" for the runtime that turns a model into an
> agent; this repository is the human-side discipline that sits on top of any
> such runtime.) Calibrated on one person's workload; read the core claim below
> before reusing any number.

## What this is, in the author's words

- **It is how I work with AI agents every day** — when I hand work off, how I
  check what comes back, and how the rules themselves change as I learn.
- **It is a method, not code.** Nothing here explains how any agent runtime or
  SDK works. The seven files never name a vendor — whichever agent you use,
  whichever model you switch to, it still applies.
- **It is a way of operating and a set of suggestions, and you still have to do
  the work yourself.** The thresholds in these files are the ones I calibrated
  on my own workload. Yours have to come out of your own runs.
- **It passes on a road already walked.** The lessons are evidence of what went
  wrong and what I changed — read them as evidence, not as instructions.

A small set of rules that one person calibrated, over months of daily use, for
delegating work to AI agents: when the main conversation must hand work off,
which capability tier gets it, what a dispatch prompt must contain, how output
is verified, and how the rules themselves are allowed to change.

---

## The core claim (read this before anything else)

**This is a system calibrated by one person in one environment. What is
portable is the method — the shape of a criterion, the structure of
verification, the moment a rule becomes worth writing down. The constants are
not portable.**

Every number in these files (three files, three hundred lines, thirty lines of
report, twenty lessons, two retries) is the output of a calibration run against
one workload, one toolchain, one person's tolerance for interruption. (These
are the thresholds the rules use — the delegation trigger, the report cap, the
pruning point for the lesson log, the retry limit — not a count of what is in
this repository; `lessons.md` holds eleven cases.) Those
numbers are evidence that a threshold existed and was worth finding. They are
not evidence that this particular threshold is yours.

The correct way to use this repository is to run the same method in your own
environment and arrive at your own constants:

1. Do the work without the rule and watch where it goes wrong.
2. Find the signal that appeared *before* it went wrong.
3. Write the rule as **trigger signal to action**, with one positive and one
   negative example drawn from what actually happened to you.
4. Set the threshold where your evidence puts it, not where this repo puts it.
5. Wait for the same class of failure to happen twice before promoting a note
   into a rule.

Copying is fine — most people learn by copying first. What matters is where
you copy to: into your own sandbox, where you understand the ideas, run the
method once, and then adjust the numbers to your own work step by step. Do not
drop these files straight into your live workflow. That puts a process you
have not yet tested on yourself into production, and it is worse than having
no rules: rules that fire wrongly teach people to route around the rulebook.

---

## What is in here

| File | What it covers |
|------|----------------|
| `model-dispatch.md` | When the main conversation must delegate, which tier receives the work, the reporting contract, escalation, verification, cost intuition |
| `judgment-rubrics.md` | Five decision rubrics an unreliable model can execute: escalate, done, ask, change course, quality floor — plus an honesty clause about what none of this fixes |
| `delegation.md` | Five prompt templates (search, build, refactor, research, review) and the mistakes that make a dispatch fail |
| `governance.md` | How the rules themselves may change: permission tiers, the governance-file red line, the four-step edit, pruning thresholds, and the four ways a system like this dies |
| `lessons.md` | Eleven concrete failures that produced the rules. Read as evidence, not as instruction |

Read `model-dispatch.md` first; it is the file you will use every day.
`governance.md` is the file you will need later, and it is not optional: rules
accumulate, and accumulation by itself is fine — what rots a system like this
is accumulation with no rule for tidying up. `governance.md` is that tidying
rule.

## Model tiers

The files never name a vendor or a model. They use three tiers, and you map
them onto whatever you actually have:

| Tier | For |
|------|-----|
| **Mechanical** | Format checks, list comparison, applying an already-defined pattern in bulk, single-file summaries |
| **Standard** | The default. Implementation, extraction, web research, ordinary review, scanning |
| **High-judgment** | Hard debugging, cross-file architecture, second opinions, adversarial review, high-stakes calls |

The tier boundaries move every time the underlying models change. The rule that
does not move is: **decide the tier explicitly on every dispatch, never inherit
it by default.** Inheriting means expensive models doing trivial work and cheap
models doing consequential work, and you find out weeks later.

## What this is not

- Not a framework. There is nothing to install and no code to run.
- Not benchmarked. Nothing here was measured against a control group. It is one
  operator's field notes with the failures still attached.
- Not stable. Roughly half of these rules exist because a specific tool behaved
  a specific way; when the tool changes, the rule should be deleted, not
  preserved out of respect.
- Not complete. `judgment-rubrics.md` ends with an explicit list of the things
  this method cannot fix — ambiguous requests and matters of taste. Those are
  handed back to a human by design.

## The few things that probably do transfer

If nothing else survives contact with your environment, these might:

- **The commander does not do the work.** Whatever your context budget is,
  detail that the main conversation does not need should never enter it.
- **Whoever did the work does not verify the work.** Verification goes to a
  reviewer with no memory of how the thing was made.
- **A command exiting successfully is not evidence that anything happened.**
  Anything written to an external system gets an independent read-back.
- **Legislate on the second occurrence, not the first.** One incident produces
  a note; a repeat produces a rule. Otherwise the rulebook grows faster than
  anyone can read it, and an unread rulebook is the same as no rulebook.
- **A rule that only says what to do gets guessed at.** A usable instruction is
  behavior plus the reason plus a concrete counter-example.

Notice that all five are structural. None of them contains a number. That is
the actual dividing line between what travels and what does not.

## Provenance and honesty

These rules were extracted from a private operations repository and rewritten
for public reading. Names, paths, project identifiers, dates, vendors, and
figures have been removed or reduced to magnitudes. The examples are real
events, described abstractly enough to be publishable — an example was kept
abstract rather than deleted, because a rubric without an example is just a
slogan, and if abstraction destroyed the example's meaning the rule was left
out entirely.

Some of the sharpest material is deliberately absent: the security recheck
procedure and the environment's known dead ends were both too revealing of one
specific machine to publish, and neither would have been useful to you anyway.
That is the same point the core claim makes, in a different form.

**Admitting that something does not generalize is more credible than claiming
it holds everywhere.** What is worth trusting here is the reasoning. What is
not worth trusting is the numbers.

## License

MIT. See `LICENSE`.
