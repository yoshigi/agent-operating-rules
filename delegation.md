# Delegation Templates

> How to use: copy the matching template, fill the `<>` slots, and pass it as
> the dispatch prompt. Tier suggestions are on each heading; when unsure, see
> `model-dispatch.md` §2.
>
> The reporting contract is already built into every template: conclusions plus
> `file:line`, long artifacts written to a file, ~30-line cap, no fabrication.

---

## T1. Search and scan — read-only agent, Standard tier (Mechanical if it is pure pattern matching)

```
Goal: find <what>.
Motive: <what the main conversation will do with the result — this is what
        tells you which way to lean at a fork>.
Scope: <directory / repository / filename pattern>. Search breadth: <moderate | very thorough>.
Acceptance criteria:
- List every hit as "path:line — one sentence of explanation"
- State explicitly which areas you covered and what you did not find
  (not finding something is also an answer)
Report format: a list, 30 lines maximum. Do not paste long extracts.
If you find nothing, say so and list what you covered. Do not guess.
```

## T2. Build — full-tool agent, Standard tier

```
Goal: <what to produce, one sentence>.
Motive: <how this output will be used>.
Input: <paths to read / where the spec lives>.
Constraints: <files that must not be touched / formats that must be followed /
             hard rules, e.g. source material is read-only>.
Acceptance criteria (check these yourself before reporting):
- <checkable condition 1, e.g. file X exists and contains sections A, B, C>
- <checkable condition 2, e.g. every internal link resolves>
- For anything written to an external system, state what was written where.
  Final confirmation is run by the main conversation with an independent read
  command (model-dispatch.md §6) and cannot be delegated.
Report format: write the artifact to a file. Return the path, a three-line
summary, and the acceptance criteria ticked off one by one. 30 lines maximum.
Anything you could not do, mark as not done with the reason. Do not pad with
half-finished work.
```

## T3. Refactor and batch edits — full-tool agent (Mechanical if the pattern is fully defined, Standard if judgment is needed per file)

```
Goal: change <files in scope> from <old pattern> to <new pattern>.
Motive: <why>.
Pattern definition (important — follow this, do not improvise):
- Old: <a concrete example of the old form>
- New: <a concrete example of the new form>
Scope: <file list or glob>. Nothing outside this.
Acceptance criteria:
- After the change, a search for '<a marker of the old pattern>' returns zero
  hits within scope
- <evidence that behavior did not change, e.g. a test passes, a command's
  output is unchanged>
Report format: list of changed files (path plus one sentence each), 30 lines
maximum. For anything that does not fit the pattern definition, skip it and
list it. Do not widen the interpretation on your own.
```

## T4. Research — full-tool agent, Standard tier (High-judgment if the conclusion drives a major decision)

```
Goal: answer <a specific question>.
Motive: <what the answer will decide>.
Source priority: <official documentation > existing notes in this repository >
                 general web>. Tag every conclusion with its source URL or path:line.
Acceptance criteria:
- The direct answer to the question, stated first
- An evidence list (source plus one sentence each)
- A clear separation between verified fact and your inference. Label inferences
Report format: 30 lines maximum. Long research goes to a file at <path>; return
the path plus a three-line conclusion.
If you cannot find it, report that plus which sources you tried. Do not
substitute recall for verification.
```

## T5. Review and acceptance — full-tool agent, Standard tier (High-judgment when the stakes are high). **Fresh context is mandatory — do not feed it the producer's conversation.**

```
Role: you are the reviewer. You have no relationship to the producer and owe
      them no politeness.
Goal: verify whether <artifact path> satisfies the conditions below.
Background: <what the artifact is and who uses it — with no account of how it
            was produced, and no justifications>.
Acceptance criteria (pass/fail each, plus one line of reasoning):
- <condition 1>
- <condition 2>
- Standard check: do the referenced paths, tool names, and version identifiers
  actually exist? Verify them; do not accept what merely looks plausible
- Standard check: are any rules contradictory, or phrased ambiguously enough
  that a weaker model would misread them?
Report format:
- One-line verdict: PASS / FAIL (with the failure count)
- Failure table: condition | location (path:line) | problem | suggested fix
- 30 lines maximum; write to a file and return the path if longer
Report only. Do not fix anything — fixing belongs to whoever dispatched you.
```

---

## Common mistakes — check before dispatching

| Mistake | Consequence |
|---------|-------------|
| No motive given | The agent guesses at every fork and returns something that answers a different question |
| Acceptance criteria written as "ensure good quality" | Equivalent to writing nothing. Criteria must be sentences you can tick off |
| The instruction states the behavior but omits the reason and a counter-example (e.g. "be concise", with no "because the reader never saw your process; do not use arrow chains; do not invent abbreviations") | The model can only guess how good is good enough, and every model guesses differently. A usable instruction is **behavior plus reason plus concrete counter-example** |
| Pasting a large slab of the main conversation into the subagent | Cold-start cost explodes. Give it only the paths and the spec it needs |
| The reviewer is the producer (one agent verifies its own work) | Self-verification is no verification. See `model-dispatch.md` §6 |
| Forgetting to specify the tier | It inherits, and trivial work runs on an expensive model |
