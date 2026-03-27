# CC Onboard

> A new era of open source — not open code, but open conversation.
>
> *code is cheap, show me the talk.*

---

I can't write code.

For a long stretch of using Claude Code, my approach was: don't understand it, approve it anyway, see what happens.

AI was helping me do a lot of things. But something was always off. It would modify files I didn't ask it to touch, reinvent wheels that already existed, hallucinate, explain things at length when I needed none of that. I assumed it was a model problem. It took me a while to realize: **I had never told it who I was.**

Every conversation, it started from zero, guessing. It didn't know I hated filler. Didn't know I'm idea-driven and like to think out loud. Didn't know I always want the MVP, never the complete solution. Didn't know my identity is in taste and judgment, not technical skill. It didn't know any of this, so it gave me an average.

Fixing that took a long time. Not technically — cognitively. You have to understand yourself before you can write a useful configuration. It's not filling out a form. It's self-examination.

Eventually I wrote a `CLAUDE.md`. After that, AI started actually working for me.

Then I realized: this might not be my problem.

The people around me who use AI most seriously mostly can't write code. They know "if you don't set it up right, it doesn't matter how good the AI is" — but nobody tells them how to set it up. They install Claude Code, find it ships fully armed — rules, hooks, permissions, configuration stacked on configuration — light users burn tokens on nothing, serious non-technical users can't find a foothold. They use it for a while, decide it's "fine," and stay there.

Maybe you're the same: used a lot of AI, never actually run an agent. Every session you re-explain who you are, what you're working on, what style you want. It helps, but something's always slightly off, and you can't name what.

You know you're supposed to configure it. You just don't know where to start. So you searched, found someone's CLAUDE.md on GitHub, figured it looked reasonable, copied it. Then noticed it still felt slightly wrong — of course it did. That was their answer. The question is yours.

What's off is it doesn't know you. Not because you didn't try. Because nobody in this process ever asked you the right questions.

This skill is the path I paved after walking that road myself.

---

## Is This You?

I have a name for this kind of person: **can't code, but insists on being in charge.**

You don't write code, but you have your own ideas and taste. Your identity lives in judgment, not in technical skill. You think from first principles. You want a sparring partner, not an obedient executor.

You move first and ask questions later. You find direction through divergence. You always want the MVP, but it has to make you say "wow." You enjoy the exploration, but you know when to stop.

The four things that annoy you most about AI: **unauthorized changes, reinventing wheels, fabrication, and yapping.** You're not picky — these are the basic trust boundaries between you and AI.

If you're reading this and thinking "that's literally me" — this skill was written for you.

---

## There Are Two Ways to Use AI

**AI that works for someone else**: answers your question, resets to zero, has no idea who you are by the next message. You think you're using AI. You're feeding it random prompts.

**AI that works for you**: knows your rhythm, knows you hate filler, knows you'd rather move fast and correct later. Every session builds on the last. You're directing — not accommodating.

This skill does one thing: **it makes that switch.**

---

## This Is Your Entry Into the Agent Era

You've probably used a lot of AI tools. ChatGPT, Copilot, Cursor, a dozen wrappers.

But you may not have actually entered the agent era yet.

The agent era isn't "AI got smarter." It's **you learning how to delegate your will to a machine**. Not you adapting to it — it adapting to you. That requires configuration, a starting point, and someone asking you the right questions — then translating your answers into something a machine can act on.

`CLAUDE.md` is that configuration. It's the personal contract between you and Claude Code. With it, Claude knows who you are. Without it, it only knows what you typed.

CC Onboard writes that contract for you.

---

## What It Does

**Interviews you.** 8-10 questions — not multiple choice, not a scale, but specific scenario prompts. "What's the most frustrating thing AI has ever done to you?" not "What response style do you prefer?" Real preferences only surface in real situations.

**Generates your `CLAUDE.md`.** Based on what you actually said, in your voice, placed at `~/.claude/CLAUDE.md`. It also reads your personality to decide whether to add safety hooks — are you a "move fast, ask later" type or a "confirm every step" type? It figures it out and configures accordingly.

**Recommends and installs skills.** Matched to your actual use cases. Not a catalog you scroll through alone.

**Three depth tiers — your call:**
- **Quick Start** — Just the CLAUDE.md. 5 minutes. Works immediately.
- **Balanced** — CLAUDE.md + a few skills + optional hooks. Good daily driver.
- **Full Setup** — Everything. Skills, hooks, memory. For heavy users.

---

## How It's Different

Three existing projects, all found in the wild:

- **alignment-interview-cc** — 60 questions. Feels like an exam. Most people close it halfway.
- **ai-personal-os-onboarding** — 4-round dialogue, but bundled with a course framework.
- **gsd:profile-user** — Passively analyzes your conversation history. Doesn't actively guide you.

The difference here comes down to one line: **it helps you discover who you are, instead of asking who you are.**

A questionnaire gives you options and you pick the closest one. A follow-up conversation forces you to describe a situation — and your real self comes out.

We also tried branching choice trees — generating the next question's options based on your previous answer, branching infinitely. Too heavy. Too much logic. And the fundamental problem with choices remains: people rush through them, skip, pick "close enough." Only when you describe a specific situation in your own words do your real preferences surface.

---

## Install

```bash
npx skills add slr/cc-onboard -g -y
```

## Use

First time:
```
/onboard
```

After you've been using Claude Code for a while:
```
/onboard review
```

No `CLAUDE.md` detected → full flow, 8-10 questions.

`CLAUDE.md` already exists → review mode, 3-4 targeted questions, proposed changes only. Nothing overwritten without your explicit OK.

All data stays local. Nothing leaves your machine.

---

## Why CLAUDE.md Needs Maintenance

Your first CLAUDE.md will be wrong. Not completely — just incomplete.

Use it for a week and you'll find: "I also hate when it does X" or "actually I do want it to explain sometimes." That's normal. You're calibrating against a system, and the first pass is never fully accurate.

`/onboard review` is designed for exactly this. Think of it as a quarterly retrospective, not a one-time setup. Every review closes the gap between who you think you are and how you actually work.

### The Ideal vs. Reality

The ideal would be an AI that silently observes your usage patterns and optimizes your configuration automatically. An invisible coach.

But in practice, this goes wrong easily — the moment it changes your config on its own and you notice, your first instinct is to turn it off. Because people who "can't code but insist on being in charge" have one core need: **I decide.** Any unsolicited move, no matter how well-intentioned, triggers distrust.

So we chose the least clever but most reliable approach: **you come to it, it responds.** Build A (guided onboarding), but design it to be re-runnable — which effectively achieves C (periodic coaching). Control stays in your hands, always.

### By the Way, CLAUDE.md Has Three Layers

Claude Code's configuration actually works on three levels:

- **`~/.claude/CLAUDE.md`** — Your personal contract with AI. Applies across all projects. This is what CC Onboard generates.
- **`~/CLAUDE.md` or project root** — Project-specific rules. Like "this project uses Python" or "deployed on AWS."
- **Subdirectory `CLAUDE.md`** — Even more granular context.

You only need to worry about the first layer to start. As you get comfortable, you'll naturally know when you need the second.

---

## The Spirit Behind This

This skill was itself built through conversation.

It started with a brainstorm about how to combine three different AI tool frameworks into one personal workflow. During that process, AI used follow-up questions to help me discover my own usage preferences and pain points. Then I realized: the "follow-up → self-discovery" process itself is a product.

How the idea surfaced, which directions got rejected, what the final decisions were and why — that process is more valuable than the code. So it's open-sourced too, in `archive/`.

A new era of open source — not open code, but open conversation.

Take the code, you can copy the tool. Take the conversation, you can see how the tool was thought into existence.

---

## License

MIT
