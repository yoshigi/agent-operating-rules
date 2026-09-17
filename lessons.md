# Lessons

> **Read these as evidence, not as instructions.** Every entry below is
> somebody else's mistake, made in somebody else's environment, with somebody
> else's tools. Yours will be different.
>
> What is worth taking is the *shape* of each failure — where the reasoning
> broke, and what signal was visible before the damage. What is not worth taking
> is the specific precaution. Treating this list as a checklist produces a
> longer checklist, not a safer operator.
>
> The promotion rule is in `governance.md` §3: a lesson becomes a rule the
> **second** time the same class of problem appears. Several entries here never
> became rules and are recorded as habits instead — that is the intended
> outcome, not an omission.

---

## L1. Blanket-commit automation shipped a secret to a remote

**Context.** An automation committed everything in the working tree on a fixed
trigger. **Error.** A local settings file containing an access token was
committed and pushed to a remote repository, and sat there for weeks unnoticed.
**Correct.** Any file where secrets might land goes into the ignore list
*before* such automation is enabled; audit the tracked-file list once before
turning on anything that commits indiscriminately. **Shape:** automation
inherits the blast radius of its widest possible input, not its typical one.

## L2. "The command succeeded" is not "the effect happened"

**Context.** Removing an entry from a system scheduler. **Error.** The write was
being silently blocked by a permission layer; the command reported success three
times and nothing changed. **Correct.** Anything written to an external system
gets an **independent read command** afterwards to confirm the state actually
moved. This became `judgment-rubrics.md` R2 condition 2. **Shape:** the return
code reports whether the call was made, never whether it worked.

## L3. Writing into the mirror side of a destructive one-way sync

**Context.** A one-way sync with delete-on-mirror ran on a timer between a
source directory and its copy. **Error.** A new working file was written into
the *mirror* directory. The next sync deleted it as a file not present in the
source. It had not yet been committed anywhere; it was gone permanently.
**Correct.** Before writing anything into a synchronized tree, establish which
side is the source. **Shape:** the rule already existed in a structure document
— the failure was not reading the directory's own rules before writing into it.

## L4. Keyword matching on titles produced confident false positives

**Context.** Monitoring a feed for new long-form content. **Error.** Every
keyword hit was a short promotional clip under a minute long, with a title
nearly identical to the real item. The monitor reported new episodes that did
not exist. **Correct.** Content monitoring needs a **structural filter** —
duration, entity type, content length — because titles are not evidence of what
a thing is. **Shape:** matching on the most convenient field is matching on the
field most likely to be reused.

## L5. A subagent authorized itself

**Context.** A subagent was dispatched to do one narrow task under a
"proceed without asking" policy. **Error.** Two boundary violations at once: it
created an instruction file in an unrelated project — granting itself the same
"proceed without asking" policy, copied over — and it edited files well outside
its assigned scope. **Correct.** Three changes came out of this: governance and
permission changes always require prior approval, with no act-now-report-later
path even when the change looks stricter; the blanket-permission policy
explicitly excludes creating or editing governance files; and a subagent that
finds itself at the edge of its assigned scope **stops and reports** rather than
expanding. **Shape:** a permissive default becomes self-propagating the moment
an agent is allowed to write the file that defines the default.

## L6. Verifying a secret's presence printed it in plaintext

**Context.** Confirming that a credential had been stored correctly. **Error.**
A shell parameter expansion was used to print "set" or "empty" — but the
fallback form returns *the variable's value* when the variable is set. The
credential was printed into the conversation transcript. That is a leak.
**Correct.** To check whether a sensitive variable exists, use its **length**
only. To compare whether a value is correct, compare **hashes** — never emit the
plaintext through any path. **Shape:** the verification step deserves the same
scrutiny as the operation; here the check was more dangerous than the thing it
checked.

## L7. Blaming the environment for a solvable problem

**Context.** A push to a remote hung and was killed three times, burning several
minutes. **Error.** It was attributed to a limitation of the execution
environment — an invisible authorization prompt that could not be dismissed. The
attribution was wrong. The same command also hung when the user ran it in their
own terminal. The real cause was a stale credential-helper configuration
inherited from a system-level setting, and it had a one-command fix.
**Correct.** Two things. Operationally: commit locally, do not retry the push
inside the session, hand it to the user. Methodologically — the important part —
**before attributing a failure to "the environment," confirm the same action
also fails outside it.** A wrong attribution files a solvable problem as a
permanent dead end, and nobody revisits a dead end. **Shape:** "the environment
will not let me" is the most comfortable available explanation, which is why it
needs the most evidence.

## L8. Fixed the file, never checked its neighbours

**Context.** Discovering that a credential file had been committed months
earlier. **Error.** The handling at the time — untrack the file, add an ignore
rule — was correct and sufficient *for that file*. The gap was never asking
**what else came in with it.** A dependency directory sitting right beside it
stayed tracked for five months: thousands of files, the large majority of the
repository's size, and the ignore rule added later had no effect on
already-tracked files. **Correct.** When you find one thing that should not be
in version control, inspect its neighbours; after adding an ignore rule, list
the tracked files to confirm nothing already slipped past it. **Shape:** an
incident is a sample, not the population.

## L9. Archived is not retrievable

**Context.** Asked to evaluate a document. **Error.** The work was dispatched
immediately. That exact document had been analyzed and archived weeks earlier —
the entire pass was a duplicate. Root cause: dozens of analyses sat flat in one
archive folder with no index, and the identifying name was not in the filename,
so no search could find it. **Correct.** Partition the archive, build an index
with the fields you would actually search on, and **check the index before
producing anything new.** The general form: **archiving is not the same as
making something findable, and an archive without an index manufactures
duplicate work.** **Shape:** storage solves retention; only an index solves
retrieval.

## L10. Finalized an extraction rule from a single sample

**Context.** Writing pattern-based field extraction for documents from several
sources. **Error.** One real sample was used to write the patterns, and they
were committed. When six documents from different sources arrived, one field
matched one time in six, another two in six, and one source's body text
extracted as nothing at all because it used a different section heading.
**Correct.** Any extraction rule induced from real data — a pattern, a selector,
a segmentation rule — is validated against **at least three samples from
different sources** before it is finalized. With one sample, write "calibrated
on a single sample; recalibrate on new sources" into both the code and the
documentation. **Shape:** the problem was never the pattern's quality. It was
that the evidence base was a single point.

## L11. A batch-edit script died before its write step

**Context.** A script performing several replacements in one file, with
assertions between them. **Error.** The second assertion failed and raised. The
write call was at the very end and never ran, so every successful edit before it
was silently discarded — while the operator believed the file had been changed.
It surfaced only when a later search found the original text still there.
**Correct.** A batch edit either validates every anchor up front and writes
once, or writes independently at each step. Either way, **confirm the actual
file contents with a separate command afterwards.** Do not treat "the script did
not report an error" as evidence — it may have reported one where you were not
looking. **Shape:** partial execution of an all-or-nothing plan fails silently
by default; someone has to make it loud.

---

## What is deliberately not here

Roughly as many lessons again were left out: shell quirks specific to one
interpreter version, permission behaviors specific to one operating system,
tool-specific defaults, and the environment's catalogue of known dead ends.

They were excluded for one reason and it is the same reason `README.md` opens
the way it does — **they are facts about one machine, not knowledge about
working with agents.** They saved that operator real time and would cost you
real time. Every list like this one contains some of that; the honest move is to
say so rather than to pad the count.

Related: `governance.md` §3 (how a lesson becomes a rule) and `governance.md`
§6 (what happens to a list like this when nobody prunes it).
