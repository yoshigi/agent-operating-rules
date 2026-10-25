# Agent Operating Rules

**In one line:** this repository shows how one person hands work to AI agents, checks what the agents hand back, and keeps the rules from rotting. The method is for people, not code.

---

## Start with a small story

Say you ask an AI agent to cancel a task that runs every morning. It replies "done." You ask two more times. Both times it says "done." The next day, the task runs anyway.

So where did it go wrong? The agent wasn't lying. A permission setting was quietly blocking the change, and "done" only meant the request had been handled on the agent's side. Nothing in the world had actually changed.

Most problems with AI agents look like this. The work *seems* finished, and nobody checks. This repository is the set of habits I built so that I check, every time, without having to think about it.

## Think of this repository as onboarding notes

If a new teammate joined you tomorrow and started handing work to AI agents, what would you tell them on day one? It would be these three things:

1. **When to hand work off, and to whom.** Not everything should be delegated, and not every job needs the most expensive model.
2. **How to check what comes back.** "Done" on the screen is not done in the world.
3. **How to change the rules when you learn something.** Without a way to tidy up, rules pile up until nobody reads them.

The files here answer those three questions, one file per question.

## Who this is for

- **You use AI agents every day** and want fewer "it said done, but it wasn't" moments.
- **You're bringing AI into a team** and need a shared way to decide what to delegate and how to sign off on the result.
- **You teach or advise others on AI adoption** and want real examples of what goes wrong, with what was changed afterwards.

You don't need to write code to use any of it.

## What's in here

| File | The question it answers |
|------|-------------------------|
| `model-dispatch.md` | When should I hand this off, to which level of model, and what should the agent report back? Read this first. It's the one you'll use every day. |
| `judgment-rubrics.md` | How do I decide when something is really done, when to ask a human, when to stop and change course? Five checklists, plus an honest list of what they can't fix. |
| `delegation.md` | What should I actually write when I hand off a task? Five ready-to-copy templates, and the mistakes that make a handoff fail. |
| `governance.md` | How are the rules themselves allowed to change? You'll need this later, and it isn't optional: rules piling up is fine, but rules piling up with no way to tidy them is what kills a system like this. |
| `lessons.md` | Eleven real failures behind these rules and habits, and what changed after each one. Some became rules, some stayed as habits. Read them as evidence. |

### Model levels: pick the right person before you hand off

An AI company usually offers several models. Some are fast and cheap. Others are slower and cost more, but think more deeply. Choosing a model level just means deciding, before you hand off a task, which kind of model should get it.

Think of how work is split up at a company. Simple checks go to a new hire, most everyday work goes to an experienced staff member, and only the hard calls go to a senior advisor. The senior advisor costs the most per hour, so asking them to check formatting is a waste. And if you hand an important decision to the new hire, you won't notice right away when it goes wrong.

The rule files describe this with three levels and never name a vendor, because models keep changing. You map them onto whatever you use:

| Level | Like a company's | Give it |
|------|------------------|---------|
| **Mechanical** | New hire | Format checks, comparing lists, applying a pattern you've already defined |
| **Standard** | Experienced staff | Most work: building, extracting, research, ordinary review |
| **High-judgment** | Senior advisor | Hard debugging, second opinions, high-stakes decisions |

**One example: Claude's four models, the official positioning and how I actually use them** (it will go out of date as models change; it's only a starting point)

| Level | Model | Anthropic's description | How I actually use it |
|------|-------|----------------------------------|-----------------------|
| **Mechanical** | Claude Haiku 5.5 | For high-volume, latency-sensitive tasks such as classification, extraction, and routing. Fastest and cheapest of the four | I rarely use it right now |
| **Standard** | Claude Sonnet 5.5 | The best combination of speed and intelligence. Fast | Agents I send out to do the work: research, first drafts, and independent review of ordinary drafts (it reviewed this README; high-stakes reviews go to the high-judgment level) |
| **Standard** | Claude Opus 5.5 | For long-running agentic coding and knowledge work. If you're unsure, Anthropic suggests starting here for most workloads. Moderate speed | The conversation where I direct the work: breaking it down, deciding who gets what, judging whether the result is usable |
| **High-judgment** | Claude Fable 5.1 | For demanding reasoning and long-horizon agentic work, or when your own tests on Opus 5.5 at higher effort still fall short. Slowest and most expensive of the four | For a while, my main conversation ran on it. Later I moved the main conversation to Opus 5.5 |

When Anthropic says to start with Opus 5.5 for most workloads, it's talking about which model to pick. When the rule files call the standard level "the default," they mean the level you pick first when handing off a task. Both land on the standard level, but they're two different things.

**Start with Opus 5.5.** Anthropic suggests starting with Opus 5.5 when you're unsure, and moving to Fable 5.1 when that isn't enough. I recommend the same. The most capable model uses a lot, and most people's subscription plans have limited usage, so making Fable 5.1 your default will use up your allowance fast. Giving it simple chores is like asking a senior advisor to sort your email. For everyday handoffs, follow the rule files: start at the standard level, move up only when it falls short, and save the most capable model for the truly hard, highest-stakes calls.

If you use ChatGPT or another vendor's models, the split works the same way: look at which models that vendor offers, then place them on the three levels from "fast and cheap" to "slower but deeper."

**Claude Code and ChatGPT are tools; levels are the models you pick inside them.** The most common way to use ChatGPT is as a chat in a browser or app. Claude Code works on your own computer, can read and edit your files directly, and can hand work off to other agents. Whichever tool you use, you can pick a level before you hand off a task.

The one rule that doesn't change when models change: **pick the level on purpose, every time.** If you let it default, expensive models end up doing trivial work, cheap models end up doing important work, and you find out weeks later.

## How to use it (try it out, then grow your own version)

Treat this section as a starting move for your workflow. First get the idea with one simple framework, then run one small experiment. You don't need to decide whether these rules are right. Make a guess, test it, and keep what actually works. Once you start trying, your own version builds up step by step.

### One framework: treat every rule as an experiment

Every rule goes through the same loop, in three moves:

1. **Make a hypothesis.** Say what you want to test. For example: "If I add one line about how I'll check the result when I hand off a task, I'll see fewer cases where the agent says done and it isn't."
2. **Test the claim.** Do it for a while and write down what actually happens, including when it doesn't help.
3. **Confirm what really works.** If the results back your guess, keep it as your own practice. If they don't, rewrite the guess or drop it.

I kept the rules in this repository one at a time using this loop, and some have since been changed or deleted. They worked for me. In your work they're still hypotheses until your own results say otherwise.

### Step-by-step, with the reason for each step

1. **Work without the rule for a while and watch where things go wrong.**

   Why: a rule written before anything has gone wrong is just a guess. Now, whenever something goes wrong, I first write down what happened and what I expected, and only then decide whether a rule should change.
2. **Find the warning sign that showed up before it went wrong.**

   Why: a rule only helps if it warns you in time. When I look back at my notes, I look for the moment something already seemed off, like an agent saying "done" without showing any proof.
3. **Write the rule as "when I see this sign, I do this," with one example where it applies and one where it doesn't.**

   Why: a rule that only says what to do leaves people guessing. Two examples show both people and agents where the edge is.
4. **Set the threshold (the point where a rule starts to be needed) where your own evidence puts it.**

   Why: the numbers in this repository, like "three files" or "thirty lines," came from my workload. They show there's a threshold worth finding. Yours has to come from your own results.
5. **Wait until the same kind of failure happens twice before turning a note into a rule.**

   Why: if every mistake adds a rule, rules pile up faster than anyone can read them. The first time gets a note. A repeat gets a rule.

### Five practices I use now

These five are how I actually work today, each with the reason I do it. The "five things you can take with you" further down are the principles you can carry over. Pick one and make it your first experiment.

- **Name the original source first.** Facts come from the original files. Anything the AI summarizes is only a reference. Why: AI can be confidently wrong. I once had an AI write "not found in the source" in its own review notes, and when I checked the original file, both passages were right there.
- **Back up before you edit.** Before changing an important file, keep a copy of the original. Why: afterwards you can compare word for word and confirm that the parts you meant to leave alone really didn't change.
- **Keep doing and checking separate.** Hand finished work to another agent that never saw how it was made. It goes through a checklist and marks each item pass or fail. Anything that fails gets fixed and checked again. Why: whoever made something is the worst at seeing its mistakes. This README went through that same review before it was finalized.
- **Write a handoff note before you stop.** At the end of each working session, write down what was decided, how far you got, and what comes next. Why: next time you start from the note instead of re-explaining everything, and there's a record of who decided what, and when.
- **Write your own opinions yourself.** Let the AI organize the material, but leave the reflections and judgment calls blank for yourself. Why: judgment is the part you're responsible for. If the AI writes it, you can't tell what you actually believe.

### One thing to try today

Pick one task you'll hand to an agent this week. When you hand it off, add one line: "When you're done, tell me what evidence shows it's really finished." Then write down the evidence the agent gives you, and whether it matches what you find when you check yourself.

A week later, look back. Did that one line save you even one "it said done, but it wasn't" moment? If yes, you have your first rule of your own. If not, you have a record that tells you where to aim your next guess.

### If you're worried

- **"I don't write code."** Every step here is a work habit. One sentence and one note are enough to start.
- **"I might set the wrong rule at the start."** Try it in your own sandbox first, meaning a practice setup that can't affect your real work. If a rule is wrong, change it or delete it. That's part of the experiment.
- **"I'm not sure these rules are right."** Treat them as hypotheses. Your results will tell you.
- **"It'll take too much time."** Start with one task and one line about how you'll check it. Look back once a week.

## Five things you can take with you (five to try first)

Start by trying these five:

- **The person in charge focuses on decisions.** Use the conversation where you direct the agent only for decisions. Send the details to the agents you hand work to.
- **Someone who didn't make it checks it.** The reviewer should be someone, or some agent, that never saw how it was made.
- **Check every success written into another system.** Anything written into another system gets checked separately afterwards.
- **Make a rule on the second time, not the first.** One incident gets a note. A repeat gets a rule. Otherwise the rulebook grows faster than anyone can read it.
- **Write what to do, why, and where it doesn't apply.** A rule people can follow says what to do, why, and shows one case where it doesn't apply.

Notice that none of these contains a number. That's the real line between what you can borrow and what you have to find yourself.

## A beginner's guide (scope and how to use it)

- **Ready to use once you've read it.** There's nothing to install. Reading it is the whole setup. The rest happens in your habits.
- **Treat each rule as what worked for one person.** Nothing was compared against a control group, meaning a second group used for comparison. Treat each rule as something that worked once, for one person.
- **When a tool changes, change the rule.** About half of these rules exist because a tool behaved a certain way. When the tool changes, delete the rule.
- **Where the rules stop, your judgment takes over.** Vague requests and matters of taste are handed back to a human on purpose. When the rules go quiet, that's your judgment being asked for.

## How this repository's content is produced (the governance method)

Every rule in this repository comes from something that really happened, recorded in my private working notes. Getting from those private notes to this public version takes six steps. The process itself applies two of the five things above: someone who didn't make it checks it, and every success written into another system gets checked again.

1. **Find the original event.** Each rule has to trace back to a real failure and what was changed afterwards. A rule with no real event behind it doesn't go in.
2. **Remove identifying details.** Names, companies, paths, dates, vendors, and exact figures come out, leaving only rough sizes, like "a few dozen lines" instead of an exact count. Before publishing, a search checks each item, and it only passes at zero matches. The one exception is the vendor and model examples in the "Model levels" section. They're there on purpose to give you a starting point, and the rule files themselves still never name a vendor.
3. **Check the abstraction.** Once an example is abstract enough to publish, every part of it still has to map back to the original event. Analogies get the same check: any detail the real event didn't have gets cut. If an example loses its meaning once it's made abstract, the whole rule stays out, because a rule with no example is just a slogan.
4. **Review independently.** A reviewer that took no part in the writing, another agent, goes through a checklist and marks each item pass or fail: does each fact have a source, do the numbers match the files, is anything identifying left behind. Anything that fails gets fixed and reviewed again. The review notes stay in my private files.
5. **Author sign-off.** I read every change and give the go-ahead before it's published. The AI never publishes on its own.
6. **Keep a record.** Every change to this public repository, and the reason for it, stays in the git history (its version log), where anyone can look it up. The original records in my private notes stay private.

That process is what lets this repository tell you plainly what doesn't generalize. What's worth trusting here is the reasoning, not the numbers.

## One calibration question to take with you

Here's what to calibrate: the last time an AI agent told you something was done, how did you know it really was?

## License

MIT. See `LICENSE`.
