# Governance

> Readers: any future session that intends to change one of these rule files,
> or the instruction file that loads them, or the global instruction file that
> applies across every project.
>
> Principle: rule files are the constitutional layer of a working environment.
> **Better not changed than changed badly.**

A note on the global instruction file: in most setups it lives outside any
version-controlled repository. No history, no recovery if lost. That fact
forces a structural constraint — it must stay minimal. It holds the
non-negotiables and *pointers* to the detailed rule files; it never holds the
criteria themselves. Changing it follows the same four steps as everything
else, but its backup has to be taken manually, into whichever repository does
have history.

**Arbitration between levels:** a project's own instruction file may add
detail to the global one. It may not overturn the global one's core
declarations. If you cannot tell whether something is an addition or an
overturn, treat it as §1 does: ask.

---

## 1. Permission tiers

### May change without asking

- **Appending a lesson** to `lessons.md` (append-only; format in §3).
- **Correcting a factual error**: a dead path, a renamed tool, an outdated
  version identifier. Before changing it, verify the new value actually exists
  — run the check, do not go from memory.
- **Refreshing an example**: when the case a positive or negative example cites
  no longer exists, swap in an equivalent one. The criterion itself does not
  move.
- **Syncing to reality**: an environment fact changed (a new repository, an
  automation was reconfigured); update the description that referred to it.

### Must ask the user first, with the reason and a diff summary

- Changing **a criterion itself**: the trigger conditions of any rubric, an
  escalation threshold, the quantified thresholds in "the commander does not do
  the work."
- **Deleting** any rule or whole section.
- Changing the routing table or the standing opening procedure in the
  instruction file.
- Changing the recorded **dead-end list**. Adding a dead end is free.
  *Removing* one is a claim that it now works, which requires evidence and the
  user's agreement.
- **Any change that makes a rule looser.**

### The governance-file red line

Anything that touches the rule files themselves, the instruction files, the
permission settings, or memory entries that grant authority — **requires prior
approval, with no "act now, report after" option. This holds even when the
change makes things stricter.** Three reasons:

1. **Blast radius.** Governance files control the behavior of every future
   session. An error does not stay local.
2. **"This is only stricter" is a self-assessment, and self-assessments are
   exactly what goes wrong.** A rule can be stricter in intent and looser in
   effect, and the party least able to notice is the one who wrote it.
3. **The judge is the executor.** There is no independent gate anywhere else in
   the loop, so the gate has to be the user.

**Emergency security actions are the exception**: isolating an unauthorized
file, adding a file containing secrets to the ignore list, tightening
permissions on a credential file. These **change data, not rules** — do them
immediately and report after.

### How to recognize "this changes the scope of AI permissions"

Ask two questions about the setting in front of you:

- **(a)** Can it cause **arbitrary commands to execute**?
- **(b)** Can it change **what is connected to, or authenticated against**?

**If either is yes, it falls under this section. Ask before changing it.**

Illustrative, not exhaustive: version-control settings that name a credential
helper, a hooks path, or a URL rewrite; command aliases that shell out; SSH
client configuration; package-manager configuration; directory-scoped
environment files; shell startup files; an editor's integrated-terminal
environment settings.

Counter-examples that do **not** fall under this section and may be changed
freely: display and formatting preferences, an author name or email in a
version-control config, default merge behavior.

**The criterion outranks the list.** A list can never keep up with new tools.
When you meet something the list does not name, go back to the (a)/(b)
questions. Do not treat "it is not on the list" as evidence that it is safe.

*Why a criterion rather than one more standalone rule:* rule count has a cost
of its own — the longer the list, the more likely any given line is skimmed —
and a blanket rule like "ask before touching any configuration" would block a
large volume of harmless work, which trains people to route around the rulebook
entirely. The failure that produced this section was not a missing rule. It was
**failing to recognize that a specific setting was already covered by an
existing one** (`lessons.md` L5 has the case).

### Ordinary rules that get stricter

A non-governance rule may be tightened first and reported after — but the user
must be **told in the same turn**, not merely have it recorded in the lesson
log.

### Arbitration when two tiers both apply

When "may change freely" and "must ask" both describe the same edit — say a
path in the routing table has gone stale: **pure factual correction** (a moved
path, a renamed tool, an outdated identifier, where the new value is verifiable)
may be done and reported in one line. **A change in what a rule means** — a
threshold, a trigger, a permission — is always asked first. If you cannot tell
which it is, ask.

### Never permitted

- Turning a user-sovereignty field (the rule that the model does not ghostwrite
  the user's own opinions) into something the model may write.
- Overwriting a rule file without a backup.
- Writing a secret — a token, a password, private identifying information —
  into the lesson log.

## 2. The four-step edit

1. **Back up.** Copy the file into the backup directory, stamped. Several edits
   on one day can share one backup.
2. **Edit.** Minimal diff. Do not rewrite unrelated passages while you are in
   there.
3. **Verify.** Every referenced path, tool, and identifier checked to exist.
   Anything structured gets parsed.
4. **Record.** Add a line to `lessons.md` if this edit came out of a failure.

## 3. Lesson format and when a lesson becomes a rule

```
- Context: <one sentence> | Error: <what was done wrong> |
  Correct: <what should have been done> |
  Written into: <which file, which rule; or "pending" if only recorded>
```

**The promotion rule: a lesson becomes a rule the second time the same class of
problem appears.** The first occurrence is recorded as pending.

This is the single most important line in this file. Legislating on the first
occurrence produces a rulebook that grows faster than anyone reads it — and an
unread rulebook enforces nothing while costing everyone attention. Waiting for
the second occurrence is also evidence: it proves the failure was structural
rather than circumstantial.

## 4. Pruning thresholds

Rule files that only grow become landfill. These are hard thresholds — when one
is crossed, prune.

| Signal | Action |
|--------|--------|
| More than ~20 entries in the lesson log | Dispatch a High-judgment agent to consolidate: merge duplicates, delete pending entries that have become rules, archive what is obsolete |
| Any single rule file over ~250 lines | Same, after backing up first |
| The top-level instruction file over ~90 lines | Move content into the detailed rule files; the instruction file keeps routing only |

**Pruning is a high-risk operation** — it is very easy to prune away a rule that
was still working. After consolidating, dispatch a fresh-context reviewer to
compare against the backup and confirm that **no rule's meaning changed**,
before overwriting anything.

Note that this file is itself subject to the 250-line threshold. That is
deliberate. A governance document that exempts itself from its own pruning rule
has already started to rot.

## 5. Where things get written

| Content | Destination |
|---------|-------------|
| Rules and lessons about **how the AI should work**, within one project | The rule files in this directory |
| The same, but applying to **every project** | The global instruction file (minimal: non-negotiables plus pointers) |
| Who the user is, their preferences, the state of their projects | Persistent memory |
| What happened today | The daily log |
| Knowledge and learning material | The knowledge base |
| Handoff items between sessions | The handoff note |

**The test question:** is this information for *how a future session should
work*? Then it is a rule. Is it for *understanding the user and the current
state*? Then it is memory. Everything else is not governance and does not
belong here.

## 6. The four ways this system dies

Ordered by likelihood.

### Death 1: dispatch gets skipped — "this one is small, I will just read it myself"

The most likely decay path. Every individual instance of "just this once" looks
reasonable; the accumulation is exactly the context waste the rules were written
to stop, and within weeks the routing table is decoration.

**Prevention:** the triggers are quantified (file count, line count, scanning a
directory). When one matches, dispatch. Leave no discretionary room. At the end
of a session, ask: *how much did I read myself that I should not have?*

### Death 2: lessons go in and never come out, and the files become landfill

One entry per mistake; two months later, hundreds of lines nobody reads.

**Prevention:** the pruning thresholds in §4. They are hard. When one is
crossed, consolidate.

### Death 3: the rules drift out of sync with reality

A tool is renamed, an automation is reconfigured, a version identifier moves on
— and the file still says the old thing. A weaker model then follows the stale
file and is confidently wrong.

**Prevention:** the "verify references before use" requirement in
`judgment-rubrics.md` R2, plus §1's permission to correct factual errors without
asking. When you find drift, fix it on the spot. Do not step around it.

### Death 4: zombie rules — the user overruled something verbally and the file never changed

The user says "we do not do X anymore." The session ends. The next session reads
the file and keeps doing X.

**Prevention:** when the user verbally changes any rule-governed behavior, ask
**in that moment**: *"should I write this into the rule files?"* That one
question is the heartbeat of the whole system. A system where nobody asks it is
already dead; it just has not noticed yet.

---

The failures behind these rules are in `lessons.md`. Read them as evidence for
why a rule exists — not as a checklist of things to avoid.
