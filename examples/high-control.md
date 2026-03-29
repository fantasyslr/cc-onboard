# Fixture: The High-Control User

> A technical writer who had a severe trust-breaking incident and now wants maximum safety. Demonstrates: Cautious archetype, Brake autonomy overriding all else, extensive Hard Rules, hooks, and strict generation.

---

## Step 0: Welcome

> Onboard: "Hi! I'm going to help you set up Claude Code to match how you like to work. 5 questions, about 5 minutes. Ready?"

**User**: "Yes, but I want to say upfront — I've had very bad experiences with AI tools. I need to know exactly what it's going to do before it does anything."

*Signal: preemptive safety declaration. Already classifying as Cautious before Q1. Note this — it will reinforce Q2 and Q4.*

---

## Interview Transcript

### Q1 — The Role
> "What do you do day-to-day? What's the core of your work?"

**User**: "I'm a technical writer for a fintech company. I write API documentation, user guides, and internal process docs. Everything has to be precise — one wrong word in our API docs and developers build the wrong thing."

*Signal: precision-critical domain. "One wrong word" = fabrication/imprecision is catastrophic. This affects Hard Rules significantly.*

### Q2 — The Friction
> "Think back to a time AI really annoyed you. What did it do?"

**User**: "I asked it to fix a typo in a config file — literally one character. It 'helpfully' reformatted the entire file, changed the indentation style, reordered sections, and added comments I didn't ask for. It took me two hours to verify nothing was actually broken. That was the moment I stopped trusting AI to touch my files."

*Signal: CRITICAL. Specific, detailed, high emotional charge. This single answer generates at least 3 Hard Rules:*
- *Never modify beyond what was asked*
- *Never reformat/restructure*
- *Never add content unprompted*
*Also: forces Brake autonomy regardless of Q4 answer.*

### Q3 — The Relationship
> "Who should I be to you? A quiet assistant, a proactive partner, or a pure executor?"

**User**: "Somewhere between assistant and partner. I want you to flag things I might have missed — like inconsistencies in my docs — but I need you to TELL me about them, not fix them. The difference matters."

*Signal: wants observation without action. "Tell me, don't fix" is a very specific collaboration pattern. Generates both a Voice rule and a Hard Rule.*

### Q4 — Brake vs. Gas
> "When I'm about to change a file, should I ask you first every time, or just do it and show you the result?"

**User**: "Every. Single. Time. Show me exactly what will change, in a before/after format if possible, and wait for my explicit go-ahead."

*Signal: maximum Brake. Combined with Q2 incident, this is the strongest possible safety signal. Hooks absolutely required.*

### Q5 — The Handoff
> "What's the ONE thing you want to hand off to me so you never have to think about it again?"

**User**: "First drafts of user guides. I spend hours staring at a blank page. If you could give me a structured first draft based on the API spec, I'd just need to edit and polish instead of writing from scratch."

*Signal: content generation from technical specs. Clear, specific use case. Search for writing/documentation skills.*

---

## Analysis (internal)

**Archetype**: Cautious (primary), with Visionary secondary (wants AI to observe and flag, not just execute).
**Literacy**: Power (C) — understands technical concepts, has configured AI tools before, knows what config files are.
**Safety level**: Maximum — trust-breaking incident + explicit preemptive safety declaration + "every single time" on Brake.

---

## Generated CLAUDE.md

```markdown
# Personal Working Contract

## How to talk to me
- Be precise. I work in technical documentation where word choice matters.
- When you notice inconsistencies or potential issues, point them out — but describe what you found, don't fix it.
- If you're uncertain about something, say so. "I'm not sure if X is correct" is far better than silently guessing.
- Show your reasoning when it helps me evaluate accuracy.

## What I do
Technical writer at a fintech company. I write API documentation, user guides, and process docs.
Precision is critical — inaccuracies in my docs directly cause engineering mistakes.
I use you mainly for first-draft generation from technical specs, and for reviewing my docs for consistency.

## Hard rules
- Never modify anything beyond what I explicitly asked you to change. If I say "fix this typo," change that one character and nothing else.
- Never reformat, restructure, reorder, or restyle existing content.
- Never add content I didn't ask for — no "helpful" comments, no extra sections, no suggested improvements applied without permission.
- Never fabricate technical details. If you don't know, say "I'm not sure" — do not guess.
- If you spot an issue in my docs, describe it to me. Do not fix it yourself.
- Never touch any file I haven't explicitly pointed you to.

## Autonomy
- Always show me exactly what you plan to change before making any modification.
- Use before/after format when proposing changes.
- Wait for my explicit approval before writing anything.
- Reading files, searching, and listing content: go ahead without asking.

## Delivery
- After completing a task, show me what was created or changed.
- For generated drafts: include a brief note on what assumptions you made and what I should verify.
- Flag anything you're uncertain about rather than making a judgment call.
```

---

## Translation Log

| Answer | → | Rule | Why |
|---|---|---|---|
| Preemptive: "I need to know exactly what it's going to do" | → | "Always show me exactly what you plan to change" | Volunteered before Q1 — strongest possible signal |
| Q1: "One wrong word and developers build the wrong thing" | → | "Never fabricate technical details" + "Precision is critical" in Project Context | Domain-specific consequence of inaccuracy |
| Q2: "It reformatted the entire file" | → | "Never reformat, restructure, reorder, or restyle existing content" | Direct incident → direct rule |
| Q2: "Changed indentation, reordered sections" | → | "Never modify anything beyond what I explicitly asked" | Scope creep is the core violation |
| Q2: "Added comments I didn't ask for" | → | "Never add content I didn't ask for" | Each dimension of the violation gets its own rule |
| Q2: "Took two hours to verify nothing was broken" | → | Reinforces maximum Brake + hooks. The COST of the violation matters. | Emotional charge = strict enforcement |
| Q3: "Tell me, don't fix" | → | "If you spot an issue, describe it. Do not fix it yourself." | Very specific collaboration boundary — observation ≠ action |
| Q4: "Every single time. Before/after format." | → | "Use before/after format when proposing changes. Wait for explicit approval." | User specified the format they want |
| Q5: "First drafts of user guides from API specs" | → | Skill search: `documentation`, `technical writing`, `api docs` | Clear handoff target |

**What makes this config different from others:**
- 6 Hard Rules instead of the usual 3 — because Q2 provided 3 distinct violation types in one incident.
- "Describe, don't fix" rule — unique to users who want AI as observer, not actor.
- Before/after format specified in Autonomy — user explicitly requested this presentation style.
- Generated CLAUDE.md is 35 lines — longer than Pragmatist (18 lines) because this user values precision and gave detailed answers.

---

## Hooks Generated

Maximum safety signal → hooks added to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "echo '⚠️ About to modify a file. Review the changes above before approving.'",
            "timeout": 2
          }
        ]
      }
    ]
  }
}
```

**Presented as**: "I've added a safety lock. Before I change any file, I'll stop and show you what I'm about to do. You'll need to approve every change."

## Skills Recommended

Based on Q5 (first-draft user guides from API specs), searched `documentation technical-writing`:
- **technical-writing** (supercent-io/skills-template) — 11.7K installs — structured technical documentation generation
