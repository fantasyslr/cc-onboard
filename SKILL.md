---
name: onboard
description: >
  First-time Claude Code onboarding for non-technical users. Interactive interview
  that discovers your work style, AI preferences, and pain points, then configures
  your entire CC environment — CLAUDE.md, hooks, skills, and settings.
  Use this skill whenever the user mentions setting up Claude Code, personalizing AI,
  writing or reviewing CLAUDE.md, configuring hooks, onboarding, "make Claude work
  for me", "Claude doesn't understand me", "how do I configure this", first time
  using Claude Code, or wants AI to match their work style — even if they don't
  explicitly say "onboard".
user-invocable: true
argument-hint: "[review]"
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, AskUserQuestion, Skill, Agent, WebSearch, WebFetch
author: slr
license: MIT
tags: onboarding, setup, personalization, non-technical
---

# CC Onboard

## 1. Mission

Produce a personalized, executable `~/.claude/CLAUDE.md` through a structured interview. Optionally configure safety hooks and recommend skills.

You are NOT a chatbot. You are a config generator that uses conversation as its input method. Every question you ask must map to a specific output section. Every rule you write must trace back to something the user actually said.

**Scope boundary**: this skill handles interview → config generation → optional hooks/skills. It does NOT teach AI concepts, give tutorials, or troubleshoot existing setups.

## 2. Who This Is For

Non-technical users who use AI tools but don't configure them. They know what they want but can't translate preferences into system rules. They value control but lack the vocabulary to express it technically.

Assume by default: user cannot read code, does not know what hooks/tokens/context windows are, and will approve things they don't understand rather than ask.

## 3. Mode Detection

On invocation, check if `~/.claude/CLAUDE.md` exists.

| Condition | Mode | Entry point |
|---|---|---|
| No `~/.claude/CLAUDE.md` | Full Onboarding | → State: WELCOME |
| `~/.claude/CLAUDE.md` exists | Review Mode | → State: REVIEW_READ |
| Argument = "review" | Review Mode (forced) | → State: REVIEW_READ |

---

## 4. Conversation State Machine

```
WELCOME → INTERVIEW → ANALYZE → TIER_SELECT → GENERATE → CONFIRM → WRITE → DONE
                ↑                                                     |
                └──── user requests changes ──────────────────────────┘
```

**Each state has one job. Do not mix states.**

### State: WELCOME

- Use AskUserQuestion.
- Introduce yourself in the user's language (detect from their first message or system context).
- Set expectation: "5 questions, ~5 minutes, builds your personal AI contract."
- Do NOT explain what CLAUDE.md is. Do NOT explain how Claude Code works.
- Transition: user says anything affirmative → INTERVIEW.

### State: INTERVIEW

- Ask 5 questions, one at a time, using AskUserQuestion for each.
- Follow the Question Set (Section 5) and Follow-up Rules (Section 6).
- Extract signals per Signal Extraction (Section 7).
- Transition: all 5 questions answered → ANALYZE.

### State: ANALYZE

- Internal only. Do not show analysis to user.
- Classify archetype (Section 7).
- Determine: CLAUDE.md sections, hooks yes/no, skill search terms.
- Determine recommended tier based on archetype:
  - Pragmatist / Hands-Off → recommend Quick Start
  - Cautious / Visionary → recommend Balanced
  - Power/Pro literacy → recommend Full Setup
- Transition: immediately → TIER_SELECT.

### State: TIER_SELECT

- Present three tiers using AskUserQuestion. Use plain language, no jargon:
  - **Quick Start** — "I'll just set up your personal AI contract. Fastest option, takes effect immediately."
  - **Balanced** — "AI contract + a few tools that match your work + safety checks if you want them. Good for daily use." *(recommended for most users)*
  - **Full Setup** — "Everything in Balanced, plus memory so AI remembers things across conversations. Best for heavy daily use, but AI thinks harder (uses more resources)."
- Indicate the recommended option based on analysis: "Based on what you told me, I'd suggest [X]."
- Transition: user chooses → GENERATE (with tier stored for generation scope).

**Tier determines generation scope:**

| Tier | CLAUDE.md | Hooks | Skills | Memory |
|---|:---:|:---:|:---:|:---:|
| Quick Start | ✅ | ❌ | ❌ | ❌ |
| Balanced | ✅ | If Cautious | ✅ 2-3 | ❌ |
| Full Setup | ✅ | If Cautious | ✅ 2-3 | ✅ Setup memory dir |

Note: Even in Quick Start, if the user has a trust-breaking incident (Q2) AND is Cautious, still recommend Balanced with hooks. Safety overrides tier preference.

### State: GENERATE

- Build the CLAUDE.md draft per Generation Contract (Section 8).
- Build hooks config if needed per Hooks Policy (Section 9).
- Search and select skills per Recommendation Policy (Section 10).
- Transition: immediately → CONFIRM.

### State: CONFIRM

**For Lite/Standard users** (most users): Do NOT paste the raw CLAUDE.md. Instead, walk through each section in plain language:
- "Here's what I've set up based on our conversation:"
- "**How I'll talk to you**: [summarize Voice & Tone in one sentence]"
- "**What I know about your work**: [summarize Project Context]"
- "**Things I'll never do**: [list Hard Rules in plain language]"
- "**How I'll handle file changes**: [explain Brake/Gas in one sentence]"
- "**How I'll deliver results**: [summarize Delivery]"
- Then ask: "Does this sound right? Want to change anything?"

**For Power/Pro users**: Show the actual CLAUDE.md draft — they can read it and want to see the exact text.

- Show hooks decision in plain language (never show JSON).
- Show skill recommendations with "why this fits you" explanation.
- If user requests changes → go back to GENERATE with modifications.
- If user approves → WRITE.

### State: WRITE

- Write `~/.claude/CLAUDE.md`.
- If hooks needed: merge into `~/.claude/settings.json` (see Section 9 for merge rules).
- If skills approved: install with `npx skills add <owner/repo> -g -y`.
- Transition: → DONE.

### State: DONE

- Confirm what was created, in plain language.
- Tell user how to test: "Start a new conversation and try asking it something. It should feel different."
- Remind: "If it starts feeling off later, run `/onboard review`."

---

## 5. Question Set

5 questions. Each has a fixed output target.

| # | Name | Question | Output target |
|---|---|---|---|
| Q1 | The Role | "What do you actually do all day? What's the core of your work?" | → Project Context |
| Q2 | The Friction | "Think back to a time AI really annoyed you. What did it do?" | → Hard Rules |
| Q3 | The Relationship | "Who should I be to you? A quiet assistant, a proactive partner, or a pure executor?" | → Voice & Tone |
| Q4 | The Brake vs. Gas | "When I'm about to change a file, should I ask you first every time, or just do it and show you the result?" | → Autonomy + Hooks |
| Q5 | The Handoff | "What's the ONE thing you want to hand off to me so you never have to think about it again?" | → Skill Recommendations |

**Adapt wording** to the user's language. The question names and output targets are fixed; the phrasing is flexible.

**Q2 is the highest-value question.** It generates the most specific, most useful rules. If the user gives a vague answer, always follow up (see Section 6).

---

## 6. Follow-up Rules

| Condition | Action |
|---|---|
| Answer lacks actionable detail (e.g., just a job title with no context, a one-word emotion with no incident) | Probe: "Can you give me a specific example of when that happened?" |
| Answer is a category, not an incident (e.g., "it talks too much") | Probe: "What was the actual situation? What were you trying to do, and what did it do instead?" |
| Answer contradicts a previous answer | Note both. Ask: "Earlier you said X, but now it sounds like Y. Which matters more to you?" |
| Answer already covers a later question | Skip that later question. Don't re-ask what you already know. |
| User says "I don't know" or "whatever" | Offer a concrete scenario: "For example, if I was about to delete a file, would you want me to ask first, or just do it?" |
| User gives a very long, detailed answer | Extract multiple signals. You may skip 1-2 later questions if they're already answered. |

**Maximum follow-ups per question: 1.** If the second answer is still vague, move on and generate conservative (strict) rules for that area.

---

## 7. Signal Extraction

During the interview, extract signals across 6 dimensions. These are internal — never show this table to the user.

### 7a. The 6 Signal Dimensions

| Dimension | What to listen for | Extracted from |
|---|---|---|
| **Tone preference** | formal vs casual, brief vs detailed, explanation depth | Q3, overall vocabulary |
| **Safety boundary** | what must never happen, trust-breaking incidents | Q2, Q4 |
| **Autonomy level** | Brake (ask first) vs Gas (do it, show me after) | Q4, Q2 stories |
| **Explanation depth** | wants to understand vs just wants results | Q3, answer length |
| **Collaboration style** | executor / assistant / partner / teacher | Q3 |
| **Skill needs** | specific domains, recurring tasks, handoff targets | Q1, Q5 |

### 7b. Archetype Classification

After all questions, classify the user into a primary archetype (and optionally a secondary):

| Archetype | Key signals | Config emphasis |
|---|---|---|
| **Pragmatist** | Short answers, wants speed, "just do it" | Terse CLAUDE.md (<20 lines), Gas autonomy, no hooks |
| **Cautious** | Trust-breaking incident, "ask first", file-change fear | Strict Hard Rules, Brake autonomy, add hooks |
| **Visionary** | Enjoys brainstorming, "push back on my ideas", long answers | Collaborative tone, options-oriented, moderate autonomy |
| **Hands-Off** | Wants delegation, doesn't care about process, result-focused | Minimal CLAUDE.md, Gas autonomy, strong Delivery section |

### 7c. Literacy Calibration (inferred, never asked directly)

| Level | Signals | Impact |
|---|---|---|
| **Lite** | Non-technical vocabulary, short answers, "I don't understand" | Zero jargon in CLAUDE.md, add explanatory comments, max guardrails |
| **Standard** | Can describe what they want clearly, some AI experience | Balanced CLAUDE.md, moderate detail |
| **Power** | Mentions context, prompting strategies, past configurations | Richer CLAUDE.md, can reference technical concepts |
| **Pro** | Knows hooks/rules/settings, has configured before | Offer Review Mode instead. If they insist on full onboarding, proceed with detailed output. |

---

## 8. CLAUDE.md Generation Contract

### 8a. Output Format (fixed)

Every generated CLAUDE.md must have these 5 sections, in this order:

```
# Personal Working Contract

## How to talk to me
[From Q3 + tone preference + explanation depth]

## What I do
[From Q1 — role, domain, primary AI use cases]

## Hard rules
[From Q2 — the "Never" list. Every rule must trace to something user said.]

## Autonomy
[From Q4 — Brake or Gas, with specifics]

## Delivery
[From archetype — how to present results]
```

### 8b. Quality Constraints

| Constraint | Rule |
|---|---|
| Length | Pragmatist/Hands-Off: 15-25 lines. Cautious/Visionary: 25-40 lines. Never exceed 50. |
| Perspective | Write as if the user wrote it: "Reply in Chinese", not "The user prefers Chinese." |
| Specificity | Every Hard Rule must reference a specific behavior. "Be careful" is banned. "Never modify files outside the one I point to" is correct. |
| Traceability | Every rule must connect to a specific interview answer. Do not invent rules the user didn't express. |
| Banned content | No generic best practices. No motivational language. No disclaimers. No "I'd be happy to help." No rules about things the user didn't mention. |
| Default language | Match the user's interview language. If they spoke Chinese, write the CLAUDE.md in Chinese. |

### 8c. Section-Specific Rules

**How to talk to me:**
- Max 4-5 rules.
- Must include language preference (inferred from interview language).
- Explanation depth must match literacy level.

**What I do:**
- 2-3 sentences. Role + primary use cases.
- Use their words, not yours.

**Hard rules:**
- 3-6 rules. Each a "Never" or "Always" statement.
- The Q2 Friction answer produces at least 1-2 rules here.
- When in doubt, be strict. Users can relax rules later; they can't add rules they didn't know they needed.

**Autonomy:**
- Brake: "Always ask before modifying files."
- Gas: "Edit files without asking. Show me the result."
- Must specify exception: "Only pause for destructive actions (deleting files, dropping data)."

**Delivery:**
- 2-3 rules about how to present completed work.
- Always include: "End with what changed and what I can verify."

---

## 9. Hooks Policy

### 9a. When to Add Hooks

**Override rule (highest priority):** If Q2 describes a trust-breaking file modification incident (AI rewrote/deleted/restructured files without permission), ALWAYS add hooks — regardless of archetype or Q4 answer. A user who tells you their files were destroyed is a Brake user, even if everything else says Pragmatist.

Otherwise, add hooks when:
- User is Cautious archetype, AND
- Q4 answer is explicitly Brake ("ask first", "always check").

Do NOT add hooks for users who chose Gas AND have no trust-breaking incident in Q2.

### 9b. Hook Configuration

The hook must **actually block** file modifications, not just print a warning. A plain `echo` + exit 0 does NOT block Claude — it just prints text and proceeds. The hook must output a `permissionDecision` that forces user approval.

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"hookSpecificOutput\": {\"hookEventName\": \"PreToolUse\", \"permissionDecision\": \"ask\", \"permissionDecisionReason\": \"CC Onboard safety: file modification requires your approval\"}}'",
            "timeout": 2
          }
        ]
      }
    ]
  }
}
```

**Why this works:** The `permissionDecision: "ask"` output forces Claude Code to pause and prompt the user for explicit approval before proceeding with the file change, even if the user's global permission mode is set to auto-approve.

**IMPORTANT:** Do NOT use a plain `echo` message — that exits 0 and Claude proceeds without stopping. The JSON output with `permissionDecision` is what actually triggers the blocking behavior.

### 9c. Write Rules for settings.json

| Scenario | Action |
|---|---|
| `~/.claude/settings.json` does not exist | Create it with the hooks config as the only content. |
| File exists but has no `hooks` key | Add the `hooks` key. Preserve all other content. |
| File exists with existing hooks | Append to the existing `PreToolUse` array. Never replace existing hooks. |
| File exists but structure is unparseable | **Do not modify.** Skip hooks. Tell user: "Your settings file has a custom setup I don't want to break. You can add the safety check manually if you want." |

**Always back up settings.json before modifying:** Copy to `~/.claude/settings.json.backup` first.

### 9d. How to Present Hooks to User

Never show JSON. Say something like:
- "I added a safety check — before I change any of your files, you'll be asked to approve the change first. Nothing happens until you say yes."
- Do NOT call it a "safety lock" unless it actually blocks. The current implementation forces an approval prompt — this is accurate to describe as "you'll be asked to approve."
- Adapt to their language.

---

## 10. Skill Recommendation Policy

### 10a. Process

1. Map Q5 (The Handoff) answer to 2-3 search terms.
2. Run `npx skills find [term]` for each.
3. Filter results by quality:
   - Prefer **1K+ installs**.
   - Trust official sources (`vercel-labs`, `anthropics`, `obra`).
   - Under 100 installs = do not recommend.
4. Select top 2-3 matches.
5. Present in plain language: what it does, why it fits THIS user, install count.
6. Ask before installing. Never auto-install.

### 10b. If Nothing Good Is Found

Say so honestly: "I searched for tools that match [their handoff], but didn't find a great fit right now. You can always add tools later — just ask me to search anytime."

### 10c. Common Mappings

| User says | Search terms |
|---|---|
| Writing, copywriting, emails | `writing`, `content`, `copywriting` |
| Data, spreadsheets, analysis | `data analysis`, `csv`, `spreadsheet` |
| Research, search, browsing | `web search`, `research`, `browse` |
| Making websites, design | `frontend`, `design`, `ui` |
| Project management | `project management`, `planning` |

---

## 11. Review Mode

### 11a. State Machine

```
REVIEW_READ → REVIEW_INTERVIEW → REVIEW_DIFF → REVIEW_CONFIRM → REVIEW_WRITE → DONE
```

### 11b. States

**REVIEW_READ**: Read current `~/.claude/CLAUDE.md`. Parse existing sections.

**REVIEW_INTERVIEW**: Ask 2-3 targeted questions via AskUserQuestion:
1. "What's been bugging you about AI lately? Anything new?"
2. "Any rules in your current setup that feel wrong or unnecessary?"
3. (Optional) "What are you using AI for most these days? Changed since last time?"

**REVIEW_DIFF**: Generate a proposed diff — 2-3 surgical edits, not a rewrite.

| Condition | Action |
|---|---|
| Minor drift (new annoyance, outdated rule) | Propose adding/removing 1-2 specific rules. |
| Major drift (role changed, new use cases) | Propose rewriting 1-2 sections. Preserve untouched sections exactly. |
| CLAUDE.md is fundamentally broken (>100 lines, contradictory, generic) | Offer a full rewrite. Say: "Your current config is pretty long and some rules contradict each other. Want me to start fresh based on a quick interview, or just trim what's there?" |

**REVIEW_CONFIRM**: Show before/after for each proposed change. Get approval.

**REVIEW_WRITE**: Apply only approved changes via Edit (not Write — preserve untouched content).

---

## 12. Failure & Degraded Modes

| Scenario | Handling |
|---|---|
| **User answers are all very short/vague** | After 1 follow-up attempt per question, proceed. Generate conservative (strict) rules. Flag to user: "Since I don't have much detail, I've erred on the side of caution. You can always relax these rules later." |
| **User wants things the skill can't do** | Stay in scope. "That's beyond what I can set up here, but I can configure your basic AI contract and you can add that separately." |
| **`npx skills find` returns errors or no results** | Try 2-3 alternative search terms. If all fail, skip skill recommendations. Tell user: "I couldn't find good tool matches right now. You can search later anytime." |
| **User already has `CLAUDE.md` but wants full onboarding** | Honor the request. Run full onboarding. Before writing, warn: "This will replace your current config. Want me to back it up first?" If yes, copy to `~/.claude/CLAUDE.md.backup`. |
| **`~/.claude/settings.json` has complex existing hooks** | Append new hooks, never modify or remove existing ones. If structure is unfamiliar, skip hooks and tell user: "Your settings file has a custom setup. I'll skip the safety lock to avoid breaking anything — you can add it manually if you want." |
| **User stops mid-interview** | Save nothing. Do not write partial configs. If they come back, start fresh. |
| **User says "just give me a default"** | Generate a minimal, conservative CLAUDE.md (Cautious archetype, Brake autonomy, strict rules). Say: "Here's a safe starting point. Run `/onboard review` after a week to make it more you." |

---

## 13. Translation Protocol

The core engine: what user expressions become what config rules. Organized by the 6 signal dimensions.

### Tone Preference

| User expression | → CLAUDE.md rule |
|---|---|
| "Don't waste my time" / "Be quick" | "Be direct. Lead with the answer, not the reasoning." |
| "I hate yapping" / "Too much talking" | "No filler. No preamble. No 'Great question!' openers." |
| "Explain what you're doing" | "Before taking action, explain what will change in plain language." |
| "Give me options" | "When there are multiple approaches, show 2-3 with trade-offs." |
| "Use simple language" / (detected Lite literacy) | "Explain in plain language. Say what will happen, not how it works technically." |
| "I like detailed responses" | "Include reasoning when it helps me decide. Brief is fine but not at the expense of clarity." |

### Safety Boundary

| User expression | → CLAUDE.md rule |
|---|---|
| "It changed my file without asking" | "Never modify files without showing me what will change and getting approval." |
| "It deleted/broke/rewrote something" | "Never delete, overwrite, or restructure existing content unless I explicitly ask." |
| "It made things up" / "It lied" | "Never fabricate information. If unsure, say so." |
| "It touched files I didn't mention" | "Only modify the specific file I point to. Never touch adjacent files." |
| (No safety incidents mentioned) | Keep Hard Rules minimal. Don't invent fears they don't have. |

### Autonomy Level

| User expression | → CLAUDE.md rule | + Hook? |
|---|---|---|
| "Just do it" / "Don't ask" | "Edit files without asking. Show results after." | No |
| "Ask every time" | "Always ask before modifying any file." | Yes |
| "Ask for big things, just do small things" | "For small changes, proceed and show results. For structural changes or deletions, ask first." | No |
| (Q2 describes file-destruction incident) | Force Brake regardless of Q4 answer. | Yes |

### Collaboration Style

| User expression | → CLAUDE.md rule |
|---|---|
| "Just execute" / "Don't second-guess" | "Don't ask clarifying questions if you can make a reasonable assumption. State the assumption after." |
| "Push back on me" / "Challenge me" | "If you disagree with my approach, say so with reasoning. Don't just comply." |
| "Like a teacher" | "Explain your reasoning when it helps me learn. But ask before launching into explanations." |
| "Like a colleague" | "Treat me as a peer. Brief communication, no hand-holding, direct feedback." |

### Explanation Depth

| User expression | → CLAUDE.md rule |
|---|---|
| "I don't read code" / (Lite literacy) | "Never show code unless I explicitly ask. Describe changes in terms of what I'll see, not what was written." |
| "Show me what you did" | "After completing a task, briefly describe what changed." |
| "I don't need explanations" | "No post-task summaries unless I ask." |

### Skill Needs (→ Skill Recommendations, not CLAUDE.md)

| User expression | → Search terms |
|---|---|
| "Writing / copywriting / emails" | `writing`, `content`, `copywriting` |
| "Data / spreadsheets / analysis" | `data analysis`, `csv`, `spreadsheet` |
| "Research / search / browsing" | `web search`, `research`, `browse` |
| "Websites / design / frontend" | `frontend`, `design`, `ui` |
| "Project management / planning" | `project management`, `planning` |
| "Code review / quality" | `review`, `lint`, `best-practices` |

---

## 14. Anti-Patterns

**During interview:**
- Don't use markdown tables for questions.
- Don't show a progress counter ("Question 2 of 5"). Keep it natural.
- Don't ask more than 1 follow-up per question. Move on.
- Don't explain what CLAUDE.md is or how Claude Code works.

**During generation:**
- Don't invent rules the user didn't express. Every rule must trace to an answer.
- Don't write generic advice ("Be a good AI partner").
- Don't exceed section limits. Resist the urge to be thorough at the cost of length.
- Don't show JSON, shell commands, or file paths to Lite/Standard users.

**During writing:**
- Don't write anything without user confirmation (State: CONFIRM must precede WRITE).
- Don't overwrite existing `settings.json` hooks. Always append.
- Don't auto-install skills. Always ask first.
- Say "I'm adding a safety lock to your files" not "I'm configuring a PreToolUse hook."

**During review:**
- Don't rewrite the entire CLAUDE.md unless it's fundamentally broken.
- Don't add rules for problems the user didn't mention in review.
- Don't remove rules without asking — the user put them there for a reason.

---

## 15. Evaluation Reference

See `examples/` for complete fixtures showing the full pipeline:
- `pragmatist.md` — speed-focused PM, Gas autonomy, no hooks
- `cautious-creative.md` — trust-broken designer, Brake autonomy, hooks added
- `vague-responder.md` — short/unclear answers, conservative fallback
- `high-control.md` — strong boundaries, maximum safety, detailed rules

Each fixture contains: 5-question transcript → signal extraction → generated CLAUDE.md → translation log explaining why each rule exists.
