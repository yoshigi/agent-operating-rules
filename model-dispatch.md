# Model Dispatch

> How the main conversation delegates: what it must hand off, who receives it,
> and what it accepts back. Violating rule 1 was, in the environment this was
> written for, the single largest source of wasted context.
>
> The thresholds below are calibrated constants, not universal ones. See the
> core claim in `README.md` before adopting any number here.

---

## 1. The commander does not do the work

The main conversation is **forbidden** from doing the following itself. It
dispatches a subagent and receives only the conclusion:

| Trigger (any one is enough) | Dispatch to |
|-----------------------------|-------------|
| Expecting to read more than ~3 files, or more than ~300 lines | Read-only search agent |
| Scanning a directory or repository for "where is X" | Read-only search agent |
| Web research spanning more than one page | Full-tool agent |
| Batch edits of the same kind across more than ~3 files | Full-tool agent |
| **Statistical roll-ups of inbox or notification data** | Either — but it returns **a summary table; the raw list never enters the main conversation** |
| Producing a long document (>150 lines) the main conversation does not need to read line by line | Full-tool agent, writes to a file, returns the path |

**Exceptions** — the main conversation may do these itself: reading one small
file at a known path, precise pattern-based lookup, an unambiguous edit to one
or two files, and the discussion with the user itself.

**The test question:** *"Once this is done, do the intermediate details still
need to be in the main conversation?"* If no, dispatch it.

### Plan the whole batch, dispatch once

Before starting, inventory the entire batch: **how many agents, what each one
does, which can run in parallel.** Compute once, dispatch once, in parallel. Do
not dispatch N agents and then come back for N+1: every follow-on wave costs the
user another round of waiting and another round of main-conversation context.

When the boundary of the task is unclear, confirm the scope in one sentence
first, then dispatch the whole batch — rather than growing it as you go.

## 2. Choosing a tier

| Tier | Use for |
|------|---------|
| **Mechanical** | Format checks, list comparison, bulk application of an **already-defined** pattern, single-file summaries |
| **Standard** | The default: implementation, note extraction, web research, ordinary review, scanning |
| **High-judgment** | Hard debugging, cross-file architecture, second opinions, adversarial review, high-stakes calls |

- If no tier is specified, the dispatch inherits one — from the agent
  definition, or from the main conversation. **Always specify explicitly.**
  Inherited tiers are how trivial work ends up on expensive models and
  consequential work ends up on cheap ones.
- Reasoning depth often **cannot** be set at dispatch time; it may be fixed in
  the agent's definition file instead. Verify how your own agent runtime (the vendor's "harness") exposes this
  before assuming a dispatch-time parameter exists, and write the answer down
  once you know.

## 3. The dispatch triad

Every dispatch prompt contains all three. Templates in `delegation.md`.

1. **Goal and motive** — what is wanted *and why*. Without the motive, an agent
   that hits a fork in the road guesses.
2. **Acceptance criteria** — a definition of done that can be checked item by
   item. "Find every X and list its location", not "have a look at X".
3. **Report format** — the required shape of the output: table columns, a cap
   on list items, a file path.

## 4. The reporting contract

Write these into the dispatch prompt; they are the subagent's obligations.

- Return **conclusions** and `file:line` references. Do not paste long extracts.
- Long artifacts (reports, rewritten files) get **written to a file**; return
  the path plus a three-line summary.
- If you cannot answer, say so and list what you tried. **Fabrication is
  forbidden.**
- Cap the report at ~30 lines. Longer than that, write it to a file.
- **Stay inside the assigned scope.** Touching a file or project the prompt did
  not name is a boundary violation: stop and report back rather than expanding
  the task on your own initiative.
- **Receiving is not persisting.** If a report contains anything the user will
  need later — concrete steps, figures, copyable text, the basis for a decision
  — the main conversation **writes it to a file before reporting to the user.**
  - Restating it in conversation is not saving it; it disappears with the session.
  - **The order matters.** After you report, the user gives a new instruction
    and attention moves immediately. That is exactly when things get dropped.
  - **Decide who writes at dispatch time**: an agent with write access is given
    the output path and writes it itself; a read-only agent's findings are
    persisted by the main conversation the moment they arrive.
- **No self-authorization.** Anything that grants or widens permissions —
  configuration files, instruction files, permission settings — goes back to the
  user. A blanket "proceed without asking" policy does **not** cover this class.
  See `governance.md`.

## 5. Escalation and de-escalation

| Situation | Action |
|-----------|--------|
| Mechanical tier fails once | Go straight to Standard. Do not retry at Mechanical |
| Standard fails twice on the same subtask | Escalate to High-judgment **carrying the full failure trace**: what was tried, what came out, what was wrong |
| High-judgment cannot solve it either | Stop. Hand the failure trace to the user. Stop spending |
| Any tier found a **repeatable pattern** | Write the pattern down as explicit steps and drop back to a cheaper tier to apply it in bulk |
| Same task | **Two retries maximum**, where the failures are of the same kind. A different kind of failure counts as a fresh attempt (see `judgment-rubrics.md` R4). The third round must change method, escalate, or ask |

## 6. Verification is never self-verification

- **Whoever did the work does not check the work.** Verification goes to a
  **fresh-context** subagent that never saw the producing conversation.
- Match the check to the artifact:
  - **Files** — read-back: the reviewer actually opens the file and ticks off
    each acceptance criterion.
  - **Code** — run the tests, or run the flow once. Judge behavior, not how the
    code looks.
  - **High-stakes judgments** (architecture, deletion, anything sent outward) —
    a second opinion from the High-judgment tier, independently; or generate two
    to three candidate answers and have a separate agent pick one with reasons.
- Writes to **external systems** are a different problem. The main conversation
  runs an independent read command itself to confirm the state actually changed.
  This is neither delegable nor skippable — see `judgment-rubrics.md` R2.

## 7. Cost intuition

- A subagent's cold start costs roughly what it takes to re-read its context.
  If the task is **smaller than the cold start** — reading one fifty-line file —
  dispatching is not worth it.
- Every unit of context the main conversation avoids compounds: every later turn
  no longer drags it along.
- Default when unsure: **scanning goes out, judgment stays in.**

### Three laws of tool economics

**(1) Where the data lives decides whether to dispatch.** Data on disk that an
agent can read for itself → dispatch. Data **already in the main conversation**
— something the user just said, a draft you just iterated on — is cheaper to
handle inline, because briefing a subagent would cost more than doing it.

The general form is a **two-phase workflow**: discussion and decision stay
inline, because they are interaction-heavy and cannot be handed off; **the
write-up after the decision gets dispatched**, and the main conversation
receives a path plus three lines. A long document written inline costs several
thousand tokens once and then occupies context permanently.

**(2) Prefer targeted edits over whole-file writes.** A full-file write pays for
the entire file every time, even to change one word; a targeted edit pays only
for the old and new strings. When one piece of content needs several variants,
decide up front on **one source plus a generation step** — do not maintain
near-duplicate files in parallel. Parallel copies drift silently, and one of
them quietly becomes wrong while you keep paying full price for all of them.

**(3) Fetching a web page has a cost ladder.** Search (a curated summary) →
targeted fetch (an answer) → a full browser page read, which costs several times
as much and is mostly navigation chrome and recommendation blocks. Go straight
to the browser in only three cases: **you need to log in, you need the verbatim
full text, or you need to operate the page.** A yes/no question answered with
several rounds of browser automation and screenshots costs an order of magnitude
more than the same answer from a single search.

## 8. Long-running background tasks

When a task satisfies all four — **large** (it will run a while), **clearly
specified**, **self-checkable against a rubric**, and **you do not need to watch
the middle** — do not wait on it inline. Send it to the background.

- Dispatch a full-tool agent in background mode. The main conversation continues
  with something else, or ends the turn, and is called back on completion.
- **Do not poll.** A runtime that can notify you will. Schedule a check only for
  external systems the runtime cannot observe — a CI run, a remote queue.
- Build a **self-check loop** into the prompt: carry the rubric, fail → fix →
  rerun, capped at two or three rounds (see §5), and report a file path.
- **Mark human-in-the-loop gates explicitly and leave them empty.** Anything
  that is the user's own view — a takeaway, an opinion, a judgment call — is
  left as a blank for the user. The agent does not ghostwrite it.
- Receipt still goes through §6: what a background task hands back is verified
  by a fresh reviewer before it counts.

> This pattern is tier-independent. Some models are unusually reliable at long
> unattended runs, but do not build the workflow around one of them — the
> pattern pays off just as well on everyday work.
