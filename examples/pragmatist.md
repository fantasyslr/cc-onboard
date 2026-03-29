# Fixture: The Pragmatist

> A product manager who values speed and hates fluff. This shows the full 5-question transcript, the generated CLAUDE.md, and why each answer was translated the way it was.

---

## Step 0: Welcome

> Onboard: "Hi! I'm going to help you set up Claude Code so it works the way YOU work. Quick 5-minute chat, not a test. Ready?"

**User**: "Sure, let's go."

---

## Step 1: The 5 Questions

### Q1 — The Role
> "What do you actually do all day? What's the 'meat' of your work?"

**User**: "I'm a product manager at an e-commerce company. Most of my day is writing PRDs, analyzing sales data, and writing stakeholder updates. Lots of spreadsheets and docs."

*Signal: task-focused, output-oriented, works with data and documents.*

### Q2 — The Friction
> "Think back to a time AI really pissed you off. What did it do?"

**User**: "I asked it for a SQL query to pull last quarter's revenue by region. Instead of giving me the query, it spent three paragraphs explaining what SQL is, then gave me a query that didn't even match my schema. I just wanted the query."

*Signal: hates unsolicited explanation, wants precision, values speed over thoroughness. This is the highest-value answer — it directly becomes "Never explain unless asked."*

### Q3 — The Relationship
> "Who should I be to you? A quiet assistant, a proactive partner who pushes back, or a pure executor?"

**User**: "Executor. I know what I want, I just need it done fast. Don't second-guess me."

*Signal: strong Pragmatist archetype. Low tolerance for back-and-forth. CLAUDE.md should be terse and action-oriented.*

### Q4 — Brake vs. Gas
> "When I'm about to change a file, should I ask you first every time, or just do it and show you the result?"

**User**: "Just do it. If you break something I'll tell you."

*Signal: Gas mode. No safety hooks needed. High trust, high autonomy.*

### Q5 — The Handoff
> "What's the ONE thing you want to hand off to me so you never have to think about it again?"

**User**: "Stakeholder update emails. Every Friday I have to write a summary of what shipped, what's blocked, and what's next. Takes me an hour. I want to just say 'write the update' and get a draft I can send."

*Signal: recurring task, structured output, clear use case for writing/content skills.*

---

## Step 2: Archetype Analysis (internal)

**Primary**: Pragmatist — speed, no yapping, minimal explanation.
**Secondary**: Hands-Off — wants to delegate and get results back.
**Literacy**: Standard (B) — knows how to prompt, doesn't configure systems.

---

## Step 3: Generated CLAUDE.md

```markdown
# Personal Working Contract

## How to talk to me
- Be direct. Lead with the answer, not the reasoning.
- No fluff, no filler, no "Great question!" openers.
- If I ask for X, give me X. Don't give me X plus a tutorial on X.
- Default language: English. Use Chinese when I switch to it first.

## What I do
Product manager. I use you for PRDs, data analysis, stakeholder comms, and competitive research.
When I say "draft this," I mean production-ready, not a skeleton I have to rewrite.

## Hard rules
- Never explain something I didn't ask you to explain.
- Never add disclaimers or caveats unless there's a real risk.
- Never say "I'd be happy to help" or "Sure thing!" — just do it.
- Don't ask clarifying questions if you can make a reasonable assumption. State the assumption after.

## Autonomy
- Edit and create files without asking. Show me the result when done.
- If something breaks, fix it and tell me what happened. Don't ask what to do.
- Only pause for confirmation on destructive actions (deleting files, dropping data, pushing to prod).

## Delivery
- End with what changed and what I can verify.
- No summaries of what you just did — I can read the diff.
```

---

## Translation Log — Why Each Rule Exists

| Answer | → | CLAUDE.md rule | Why |
|---|---|---|---|
| Q2: "spent three paragraphs explaining SQL" | → | "Never explain something I didn't ask you to explain." | Direct quote from their frustration — most specific rule possible |
| Q2: "query that didn't match my schema" | → | "When I say 'draft this,' I mean production-ready" | They want output they can use, not a starting point |
| Q3: "Executor. Don't second-guess me." | → | "Don't ask clarifying questions if you can make a reasonable assumption." | Pragmatist archetype — speed over precision |
| Q4: "Just do it." | → | Full Gas autonomy section | No hooks generated |
| Q5: "Stakeholder update emails" | → | Skill search: `writing`, `content`, `email` | Recurring structured output task |

---

## Hooks Generated

None. User chose Gas mode.

## Skills Recommended

Based on Q5 (weekly stakeholder updates), searched `writing content`:
- **copywriting** (coreyhaines31/marketingskills) — 49.6K installs — structured writing assistance
