# Fixture: The Vague Responder

> A user who gives very short, unclear answers. Demonstrates the degraded mode: 1 follow-up per question, then conservative fallback. Shows how the skill handles minimal input without inventing rules.

---

## Step 0: Welcome

> Onboard: "嗨！我来帮你设置一下 Claude Code，让它更适合你的工作方式。聊 5 个问题就好，不是考试。开始？"

**User**: "好"

---

## Interview Transcript

### Q1 — The Role
> "你每天主要在做什么？工作里最核心的事情是什么？"

**User**: "销售"

*Signal: minimal. Only know the domain.*

> Follow-up: "做哪方面的销售？比如你每天花最多时间在什么事情上——找客户、写方案、谈单子？"

**User**: "主要是写方案和跟客户沟通"

*Signal: sales role, proposal writing + client communication. Enough for Project Context.*

### Q2 — The Friction
> "回想一下 AI 最让你不爽的一次，它干了什么？"

**User**: "就是回答太长了"

*Signal: category, not incident. Follow up once.*

> Follow-up: "有没有具体一次——你当时在做什么，它给了你什么让你不爽？"

**User**: "就是每次都很啰嗦吧"

*Signal: still vague after follow-up. Maximum follow-ups reached (1). Extract: hates verbose output. Generate conservative rule. Cannot generate specific incident-based rule.*

### Q3 — The Relationship
> "你希望我扮演什么角色？安静的助手、会提建议的搭档、还是纯帮你干活？"

**User**: "帮我干活就好"

*Signal: executor. Clear enough. No follow-up needed.*

### Q4 — Brake vs. Gas
> "我改你文件之前，要每次都先问你，还是改完给你看结果？"

**User**: "改完看结果"

*Signal: Gas. Clear.*

### Q5 — The Handoff
> "你最想让我帮你接手的一件事是什么？"

**User**: "写方案"

*Signal: aligns with Q1. Proposal writing is the core use case.*

---

## Analysis (internal)

**Archetype**: Pragmatist — short answers, wants speed, executor relationship.
**Literacy**: Standard (B) — can use AI, doesn't configure.
**Quality flag**: Low signal density. Multiple vague answers. Conservative generation mode activated.

---

## Generated CLAUDE.md

```markdown
# 我的 AI 工作契约

## 怎么跟我说话
- 简短直接。先给结论，不要铺垫。
- 不要啰嗦。一句能说完的不要用三句。
- 用中文回复。

## 我是做什么的
销售。主要用你帮我写客户方案和沟通内容。

## 绝对不能做的事
- 不要给我长篇大论。能短则短。
- 不要解释我没问的东西。
- 不要加免责声明或者「温馨提示」。

## 自主权
- 可以直接改文件，改完给我看结果。
- 要删东西的时候先问我。

## 交付
- 做完告诉我改了什么，一两句话就行。
```

**Note**: This CLAUDE.md is intentionally short (18 lines) because the user's interview answers were terse. Generating a detailed 40-line config from vague input would be fabrication.

---

## Translation Log

| Answer | → | Rule | Why |
|---|---|---|---|
| Q2: "回答太长了" / "每次都很啰嗦" (vague, no incident) | → | "不要啰嗦。能短则短。" + "不要给我长篇大论。" | No specific incident, but consistent signal across 2 answers. Generate category-level rule, not incident-level. |
| Q3: "帮我干活就好" | → | "先给结论，不要铺垫" + no explanation rules | Executor = don't explain, don't discuss, just output. |
| Q4: "改完看结果" | → | Gas autonomy | Clear signal, no ambiguity. |
| Q5: "写方案" | → | Project Context: "帮我写客户方案和沟通内容" | Combined with Q1 for fuller context. |
| Overall: very short answers, low detail | → | CLAUDE.md kept to 18 lines | User won't read a long config. Match their energy. |

**What was NOT generated** (and why):
- No "challenge my ideas" rule — user never asked for pushback.
- No file-safety rules beyond deletion — user didn't express file-change anxiety.
- No hooks — user chose Gas, no trust-breaking incidents.

---

## Hooks Generated

None.

## Skills Recommended

Based on Q5 (proposal writing), searched `writing content proposal`:
- **copywriting** (coreyhaines31/marketingskills) — 49.6K installs — structured writing for sales/marketing
