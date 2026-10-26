# Agent Operating Rules

**In one line:** how one person hands work to AI agents, checks what comes back, and keeps the rules from rotting. It's a method for people, not code.

---

## Start with a small story

Say you ask an AI agent to cancel a task that runs every morning. It replies "done." You ask two more times. Both times it says "done." The next day, the task runs anyway.

Where did it go wrong? The agent wasn't lying. A permission setting was quietly blocking the change. "Done" only meant the request was handled on the agent's side, not that anything actually changed.

Most problems with agents look like this: the work *seems* finished, and nobody checks. This repository is the set of habits I built so that I check, every time.

## Think of this repository as onboarding notes

A new teammate starts handing work to agents tomorrow. On day one you'd tell them three things:

1. **What to hand off, and to whom.** Not everything should be delegated, and not every job needs the most expensive model.
2. **How to check what comes back.** "Done" on the screen is not done in the world.
3. **How the rules change.** Without a way to tidy up, rules pile up until nobody reads them.

One file answers each question.

## Who this is for

- **People who use AI agents every day** and want fewer "it said done, but it wasn't" moments.
- **People bringing AI into a team** who need a shared way to decide what to delegate and how to sign off.
- **People who teach AI adoption** and want real failures, with what changed afterwards.

No coding needed.

## What's in here

| File | The question it answers |
|------|-------------------------|
| `model-dispatch.md` | Should I hand this off? To which level of model? What should the agent report back? You'll use it daily, so read it first. |
| `judgment-rubrics.md` | How do I know it's really done? When do I ask a human, or change course? Five checklists, plus what they can't fix. |
| `delegation.md` | What do I write when I hand off a task? Five templates, and the common ways handoffs fail. |
| `governance.md` | How do the rules themselves change? You'll need it later, and it isn't optional: what kills a system isn't many rules, it's having no way to tidy them. |
| `lessons.md` | Eleven real failures, each with what changed afterwards. Read them as evidence. |

### Model levels: pick the right person before you hand off

An AI company usually offers several models. Some are fast and cheap. Others are slower and cost more, but think more deeply. Model levels just mean deciding, before you hand off, which kind gets the task.

Think of a company: simple checks go to a new hire, everyday work to experienced staff, hard calls to a senior advisor. Asking the advisor to check formatting is a waste. Hand an important decision to the new hire and you won't notice right away when it goes wrong.

The rule files use three levels and never name a vendor, because models keep changing:

| Level | Like a company's | Give it |
|------|------------------|---------|
| **Mechanical** | New hire | Format checks, comparing lists, applying a pattern you've already defined |
| **Standard** | Experienced staff | Most work: building, extracting, research, ordinary review |
| **High-judgment** | Senior advisor | Hard debugging, second opinions, high-stakes decisions |

**Example: Claude's four models** (this will go out of date as models change; it's only a starting point)

| Level | Model | Anthropic's description | How I actually use it |
|------|-------|----------------------------------|-----------------------|
| **Mechanical** | Claude Haiku 5.5 | For high-volume, latency-sensitive tasks such as classification, extraction, and routing. Fastest and cheapest of the four | I rarely use it right now |
| **Standard** | Claude Sonnet 5.5 | The best combination of speed and intelligence. Fast | Agents I send out to do the work: research, first drafts, independent review of ordinary drafts |
| **Standard** | Claude Opus 5.5 | For long-running agentic coding and knowledge work. If you're unsure, Anthropic suggests starting here for most workloads. Moderate speed | The conversation where I direct the work: breaking it down, deciding who gets what, judging whether the result is usable |
| **High-judgment** | Claude Fable 5.1 | For demanding reasoning and long-horizon agentic work, or when your own tests on Opus 5.5 at higher effort still fall short. Slowest and most expensive of the four | My main conversation ran on it for a while, then moved to Opus 5.5 |

**Start with Opus 5.5.** Anthropic suggests starting with Opus 5.5 when you're unsure and moving to Fable 5.1 when that isn't enough. I recommend the same. The most capable model burns through usage quickly, and subscription allowances are limited. Giving it chores is like asking a senior advisor to sort your email. Save it for the truly hard, highest-stakes calls.

The same split works for ChatGPT or any other vendor: place their models on the three levels from "fast and cheap" to "slower but deeper." The tool (ChatGPT, Claude Code) isn't the level. The level is the model you pick inside it.

Whatever the models become: **pick the level on purpose, every time.** Let it default and expensive models end up doing trivial work, cheap models end up doing important work, and you find out weeks later.

## How to use it (try it once first, then grow your own version)

This section is a starting move: get one framework, then run one small experiment. Treat the rules as guesses to test: make a guess, test it, keep what works.

### One framework: treat every rule as an experiment

1. **Make a hypothesis.** For example: "If I add one line about how I'll check the result, I'll see fewer cases where the agent says done and it isn't."
2. **Test the claim.** Do it for a while and write down what happens, including when it doesn't help.
3. **Confirm what really works.** If the results back it, keep it. If not, rewrite it or drop it.

I kept the rules here through this loop, and some have since been changed or deleted. In your work they're still hypotheses.

### Step-by-step, with the reason for each step

1. **Work without the rule for a while and watch where things go wrong.** A rule written before anything has gone wrong is just a guess. Now, whenever something goes wrong, I first write down what happened and what I expected, then decide whether a rule should change.
2. **Find the warning sign that showed up before it went wrong.** A rule only helps if it warns you in time. When I look back at my notes, I look for the moment something already seemed off, like an agent saying "done" without any proof.
3. **Write the rule as "when I see this sign, I do this," with one example where it applies and one where it doesn't.** With the edge marked, neither people nor agents have to guess.
4. **Set the threshold (the point where a rule starts to be needed) from your own evidence.** Numbers here like "three files" or "thirty lines" came from my workload. Yours come from your results.
5. **Turn a note into a rule only when the same failure happens twice.** If every mistake adds a rule, there will be more than anyone can read.

### Five practices I use now

How I actually work today, each with the reason. Pick one as your first experiment.

- **Name the original source first.** Facts come from the original files; AI summaries are only a reference. Why: AI can be confidently wrong. I once had one write "not found in the source" when both passages were right there.
- **Back up before you edit.** Why: you can compare word for word and confirm nothing you meant to leave alone changed.
- **Keep doing and checking separate.** Hand finished work to an agent that never saw how it was made; it marks each checklist item pass or fail, and failures get fixed and rechecked. Why: whoever made it is worst at seeing its mistakes. This README was finalized that way.
- **Write a handoff note before you stop.** What was decided, how far you got, what's next. Why: you pick up from the note next time, and there's a record of who decided what, and when.
- **Write your own opinions yourself.** The AI organizes material; reflections and judgment calls are left for me. Why: judgment is the part I'm responsible for. If the AI writes it, I can't tell what I actually believe.

### One thing to try today

Pick one task you'll hand to an agent this week and add one line: "When you're done, tell me what evidence shows it's really finished." Write down the evidence it gives, and whether it matches what you find yourself.

A week later, look back. Did that line save you even one "it said done, but it wasn't" moment? If yes, you have your first rule. If not, you know where to aim your next guess.

### If you're worried

- **"I don't write code."** Every step is a work habit. One sentence is enough to start.
- **"I might set the wrong rule."** Try it first in a practice setup that can't affect real work. Wrong rules get changed or deleted.
- **"I'm not sure these rules are right."** Treat them as hypotheses and let the results answer.
- **"It'll take too much time."** One task, one line about how you'll check it, one look back a week.

## Five things you can take with you (five to try first)

- **The person in charge focuses on decisions.** Keep the main conversation for decisions; send the details to the agents you hand work to.
- **Someone who didn't make it checks it.** A person, or an agent, that never saw how it was made.
- **Check every success written into another system.** If the agent changed a schedule, a file, or a setting, open it and look.
- **Make a rule on the second time, not the first.** One incident gets a note; a repeat gets a rule.
- **Write what to do, why, and where it doesn't apply.**

None of these contains a number. That's the line between what you can borrow and what you have to find yourself.

## A beginner's guide (scope and how to use it)

- **Ready to use once you've read it.** Nothing to install. The rest happens in your habits.
- **Treat each rule as what worked for one person.** Nothing was compared against a control group (a second group used for comparison).
- **When a tool changes, change the rule.** About half of these rules exist because of how a tool behaved at the time. When the tool changes, delete the rule.
- **Where the rules stop, your judgment takes over.** Vague requests and matters of taste are left to a human on purpose.

## How this repository's content is produced (the governance method)

Every rule comes from something that really happened, recorded in my private notes. Going public takes six steps, which themselves apply "someone who didn't make it checks it" and "check every success written into another system."

1. **Find the original event.** A rule that doesn't trace back to a real failure and what changed afterwards doesn't go in.
2. **Remove identifying details.** Names, companies, paths, dates, vendors, and exact figures come out, leaving only rough sizes. A search checks each item before publishing, and only zero matches passes. The exception is the vendor examples in "Model levels," included on purpose as a starting point.
3. **Check the abstraction.** Every part of an abstracted example or analogy must still map back to the original event. If an example loses its meaning once abstracted, the rule stays out.
4. **Review independently.** An agent that took no part in the writing marks each checklist item pass or fail, such as whether facts have sources and whether anything identifying is left. Failures get fixed and reviewed again.
5. **Author sign-off.** I read it before it goes out. The AI never publishes on its own.
6. **Keep a record.** Every change and its reason is in the git history (its version log), where anyone can look it up.

That's why this repository can tell you plainly what won't generalize. What's worth trusting here is the reasoning, not the numbers.

## One calibration question to take with you

Back to the opening story: the agent said "done" three times, and the task still ran. What matters is what you take as evidence of "done." It's usually one of three:

- **Trusting the reply.** What you've confirmed is that it responded.
- **Reading its report.** One more layer, but the report is still its own; it may honestly believe it's done.
- **Checking the destination yourself.** You open the schedule or the file and see the change. Only this confirms the result itself.

If you landed on the first two, that's where your experiment can start.

Here's what to calibrate: the last time an AI agent told you something was done, how did you know it really was?

## License

MIT. See `LICENSE`.
