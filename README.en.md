# CC Onboard

> A new era of open source: not open code, but open conversation.
>
> *Code is cheap, show me the talk.*

---

I can't write code.

I have a label for myself that is embarrassingly accurate: **can't code, but insists on being in charge.**

For a long stretch of using Claude Code, my workflow looked like this: I didn't understand what it was doing, approved it anyway, and waited to see what would happen. AI helped a lot, but something always felt off. It modified files I didn't ask it to touch. It reinvented things that already existed. It fabricated. It yapped when I only wanted the answer.

I thought this was a model problem.

It wasn't.

The real problem was simpler: **I had never told it who I was.**

Every conversation started from zero. It had to guess my rhythm, my tolerance, my standards, my boundaries. It didn't know I hate filler. Didn't know I'm idea-led. Didn't know I usually want the MVP, not the full cathedral. Didn't know when I want a sparring partner and when I want a quiet executor.

So it gave me the average.

And writing a good `CLAUDE.md` turns out not to be a syntax problem. It's a self-knowledge problem. You first have to articulate: what does AI do that instantly breaks trust? When should it take initiative? When should it shut up? What is absolutely off-limits? Most people get stuck there. Not because they can't configure the system, but because nobody ever asked them the right questions.

`CC Onboard` exists for that exact moment.

It is not a template pack. It is not a survey. It is an onboarding conversation that uses 5 specific questions to surface how you actually work, then translates your answers into a personal contract that AI will actually follow.

---

## Who This Is For

This skill is for people who:

- don't write code, but care deeply about taste, judgment, and control
- use AI often, but still have to re-explain themselves every new session
- copied someone else's `CLAUDE.md` and felt it was close, but not quite right
- installed too many skills and hooks, then discovered "more" just meant slower, noisier, and more expensive
- want to enter the agent era, but don't know what the first real step is

I describe this user in one line: **can't code, but refuses to give up the steering wheel.**

That is not a flaw. It just means you don't want AI to merely answer you. You want it to work with you on your terms.

---

## What It Actually Does

`CC Onboard` does four things.

### 1. It interviews you before it configures you

You get 5 questions. Not a personality quiz. Not a giant form. Not a fake "choose the closest option" exercise.

The prompts are concrete:

- What's the most annoying thing AI has ever done to you?
- Are you more afraid of it doing too much, or too little?
- Should it act like a teammate, a teacher, an assistant, or a just-do-it executor?

Real preferences do not show up in abstract questions. They show up when you describe a real situation.

### 2. It writes your `CLAUDE.md`

Based on your answers, it generates a personal contract at `~/.claude/CLAUDE.md`.

That file is not "best practices." It is your working agreement with AI. It tells Claude:

- how to talk to you
- what your trust boundaries are
- when to move first and when to confirm
- whether you want process or just results

A good `CLAUDE.md` turns AI from a generic assistant into something that actually feels aligned.

### 3. It adds safety checks only if your style calls for them

Some people want AI to act first and report back. Others lose trust the moment it changes a file without asking.

`CC Onboard` does not force everybody into one template. It reads your answers and decides whether to add a safety check — when enabled, AI must ask for your approval before changing any file. Nothing happens until you say yes.

### 4. It recommends skills instead of sending you shopping

I fell into the obvious trap: install whatever looked powerful.

GSD, PUA, Superpowers, memory systems, enhancement packs. Each one sounded smart in isolation. Together they often meant clashing rules, token burn, and way too much machinery for simple tasks.

So this skill does not drop you in front of a massive catalog and wish you luck. It first figures out what you actually need help with, then points you to the few things that truly fit — after checking whether each recommendation is actually trustworthy (install count, source reputation).

---

## Why Follow-Up Beats Questionnaires

This is the core product decision behind the project.

Of course we considered the more "systematic" routes: lots of multiple-choice questions, or dynamically branching choice trees based on prior answers.

We rejected both.

Why?

- Multiple choice is fast, but people rush, skip, and pick "close enough."
- Branching questionnaires are clever, but heavy and brittle.
- The moment someone describes a real scenario in their own words, their actual preferences surface almost by themselves.

That leads to the one-line thesis of this project:

**It helps you discover who you are, instead of asking who you are.**

---

## Why This Is Re-Runnable by Design

The dream version is obvious: AI silently observes your behavior, learns your preferences, and keeps tuning your setup in the background.

In practice, that idea crashes into the core need of this user type:

**I decide.**

The moment AI changes your configuration on its own, even with good intentions, trust drops.

So this skill takes the less clever but more reliable route:

- Do the onboarding first.
- Do not silently take over.
- Make the process easy to run again whenever the setup starts feeling slightly wrong.

That is why `/onboard review` exists. It gives you periodic recalibration without taking control away from you.

---

## Three Depth Tiers

- **Quick Start**: just the `CLAUDE.md`
- **Balanced**: `CLAUDE.md` + a few core skills + optional safety hooks
- **Full Setup**: broader skills, hooks, and memory for heavier use

Not everyone needs the maximum configuration on day one. The best setup is not the biggest one. It is the one that fits.

---

## `CLAUDE.md` Actually Has Three Layers

One thing that is oddly under-explained in practice:

- `~/.claude/CLAUDE.md`: your personal contract with AI across projects
- `~/CLAUDE.md` or a project-root `CLAUDE.md`: machine-specific or project-specific rules
- deeper subdirectory `CLAUDE.md` files: narrower local context

`CC Onboard` focuses on the first layer, because that is the layer most people never properly define.

---

## How This Differs from Other Onboarding Approaches

I've seen a few common patterns:

- some feel like an exam
- some bundle onboarding into a larger course or framework
- some passively inspect your history instead of actively guiding you

`CC Onboard` is different for a simpler reason:

**it productizes the valuable part of the process itself.**

Not the config file. Not the framework. The follow-up conversation that keeps asking until your real working style becomes visible.

---

## Project Status

| Module | Status | Notes |
|--------|:---:|-------|
| Onboarding flow | ✅ Ready | 5-question interview + 3-tier depth selection + config generation |
| Review mode | ✅ Ready | Proposes surgical edits, not rewrites |
| Translation rules | ✅ Built-in | 6 dimensions, ~30 mappings, answer→config traceable |
| Examples | ✅ 6 fixtures | `examples/`: pragmatist, cautious, vague, high-control, visionary, conflicting signals |
| Safety checks | ✅ Ready | Real blocking (requires user approval before file changes), not decorative |
| Skill recommendations | ✅ Ready | Quality-gated (install count, source reputation) |
| Output stability testing | 🔜 Planned | Comparative testing across user types |
| Demo / screencast | 🔜 Planned | Full onboarding flow recording |

**Ready to use now.** Install and `/onboard`.

Translation rules rely on LLM comprehension rather than hardcoded logic — meaning the same answer may produce slightly different configs on different runs. If the generated contract feels off, run `/onboard review` to recalibrate.

---

## Install

```bash
npx skills add fantasyslr/cc-onboard -g -y
```

## Use

First run:

```bash
/onboard
```

When you want to review and tune the setup later:

```bash
/onboard review
```

No `CLAUDE.md` detected -> full onboarding flow.

`CLAUDE.md` already exists -> review mode, 3-4 targeted questions, suggested edits only. Nothing gets overwritten automatically.

All data stays local.

---

## Where This Project Came From

This skill did not start as a PRD.

It started as two conversations.

The first was a brainstorm about combining several AI workflow systems into a personal harness. During that process, AI kept asking follow-up questions and slowly surfaced my preferences, frustrations, and boundaries. The second conversation made the more important insight obvious: **the thing worth productizing was not some magic configuration, but the follow-up process that helps someone discover how they want to work with AI.**

So this repository tries to open-source more than the skill itself. It also open-sources, as much as possible, how the idea took shape. You can trace that arc in `archive/`.

Take the code and you can copy the tool.

Take the conversation and you can see how the tool was thought into existence.

That is what "open conversation" means here.

---

## License

MIT
