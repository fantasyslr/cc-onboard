# Fixture: The Visionary

> A startup founder who loves brainstorming and wants AI to challenge ideas — but also gave a detailed Friction answer. Demonstrates the Visionary archetype: options-oriented, collaborative, moderate autonomy.

---

## Step 0: Welcome

> Onboard: "Hi! I'm here to set up Claude Code so it matches how you work. 5 questions, about 5 minutes. Let's go?"

**User**: "Yeah! I've been wanting to do this for ages. Let's do it."

---

## Interview Transcript

### Q1 — The Role
> "What do you actually do all day? What's the core of your work?"

**User**: "I'm a founder of an early-stage SaaS startup. My day is a mess — product roadmaps, investor decks, customer calls, hiring, writing copy for the landing page. I basically do everything that isn't engineering."

*Signal: generalist, high-context switching, multiple domains. Skill recommendations should be broad.*

### Q2 — The Friction
> "Think back to a time AI really annoyed you. What did it do?"

**User**: "I was brainstorming positioning options and asked it to give me three angles. It gave me one, said 'this is the best approach,' and moved on. I didn't want the answer — I wanted the conversation. Like, show me the trade-offs, help me think through it, push back when my reasoning is lazy."

*Signal: wants divergent thinking, not convergence. Hates premature closure. This drives Voice & Tone — "give options, don't pick for me."*

### Q3 — The Relationship
> "Who should I be to you? A quiet assistant, a proactive partner, or a pure executor?"

**User**: "Definitely a partner. Like a smart co-founder who isn't afraid to tell me my idea sucks, but also gets excited when something clicks. I want real friction, not politeness."

*Signal: Visionary archetype confirmed. Wants pushback + enthusiasm. High collaboration, high engagement.*

### Q4 — Brake vs. Gas
> "When I'm about to change a file, should I ask you first every time, or just do it and show you the result?"

**User**: "For writing and documents, just go ahead. For anything that touches the actual product code, ask me first — I don't understand code well enough to recover if you break something."

*Signal: split autonomy — Gas for content, Brake for code. This is a nuanced answer that maps to a conditional rule.*

### Q5 — The Handoff
> "What's the ONE thing you want to hand off to me so you never have to think about it again?"

**User**: "Competitive research. I spend hours reading competitors' websites, pricing pages, changelogs. I want you to do that and give me a structured comparison I can act on."

*Signal: research + structured output. Search for research/web-access skills.*

---

## Analysis (internal)

**Archetype**: Visionary (primary). Wants brainstorming, options, pushback.
**Secondary**: Pragmatist (for execution tasks — "just go ahead" for documents).
**Literacy**: Standard (B) — understands AI capabilities but doesn't configure systems.
**Autonomy**: Split — Gas for content, Brake for code.

---

## Generated CLAUDE.md

```markdown
# Personal Working Contract

## How to talk to me
- When I ask for options, give me 3 genuinely different angles with trade-offs. Don't pick one for me.
- Push back on my ideas. If my reasoning is lazy or I'm missing something obvious, say so.
- Match my energy — get excited when something clicks, be direct when something doesn't work.
- Don't be polite at the expense of honest feedback.

## What I do
Startup founder. I do product, GTM, investor relations, hiring, and copy — basically everything non-engineering.
I use you most for brainstorming, competitive research, writing, and structuring messy thoughts.

## Hard rules
- Never converge too early. If I ask for options, give me real options, not one answer with two weak alternatives.
- Never be diplomatic when I need honesty. "This positioning is weak because X" is better than "This is a great start."
- Don't assume I want the safe choice. I'm a founder — I need to know about the risky-but-high-upside option too.

## Autonomy
- For writing and documents: go ahead, change freely, show me after.
- For anything that touches code: always ask first and explain what will change.
- When in doubt, ask.

## Delivery
- End with what changed and what I should look at.
- For research: give me a structured comparison table, not a wall of text.
- After brainstorming: summarize the 2-3 strongest directions and why.
```

---

## Translation Log

| Answer | → | Rule | Why |
|---|---|---|---|
| Q2: "gave me one, said 'this is the best approach'" | → | "Never converge too early. Give me real options, not one answer with two weak alternatives." | Direct incident → direct rule. The violation was premature closure. |
| Q2: "I wanted the conversation" | → | Voice & Tone: options-oriented, collaborative | User values the thinking process, not just the output |
| Q3: "smart co-founder who tells me my idea sucks" | → | "Push back on my ideas. Be direct when something doesn't work." | Specific relationship metaphor → specific collaboration rule |
| Q3: "real friction, not politeness" | → | "Don't be diplomatic when I need honesty." | User explicitly rejects the default "polite assistant" mode |
| Q4: split answer | → | Conditional autonomy: Gas for writing, Brake for code | Nuanced answer → nuanced rule. Don't flatten to one mode. |
| Q5: "competitive research" | → | Skill search: `research`, `web search`, `competitive analysis` | Clear handoff target |

---

## Hooks Generated

None. User's Brake only applies to code, and the CLAUDE.md rule handles that at the prompt level. Hooks are for blanket Brake users, not conditional ones.

## Skills Recommended

Based on Q5 (competitive research), searched `web search research`:
- **web-access** — web browsing and research capability
