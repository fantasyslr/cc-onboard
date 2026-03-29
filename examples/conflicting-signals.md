# Fixture: Conflicting Signals

> A marketing manager whose vocabulary and energy say Pragmatist, but whose Q2 incident says Cautious. Demonstrates the override rule: trust-breaking file incident always wins.

---

## Step 0: Welcome

> Onboard: "嗨！我来帮你设置 Claude Code，让它更适合你的工作方式。聊 5 个问题就好。开始？"

**User**: "嗯好，快点搞。"

*Signal: terse, impatient. Initial read = Pragmatist.*

---

## Interview Transcript

### Q1 — The Role
> "你每天主要在做什么？"

**User**: "做市场，写文案出方案，投广告看数据，没什么复杂的。"

*Signal: marketing, execution-focused, downplays complexity. Pragmatist vibe.*

### Q2 — The Friction
> "AI 最让你不爽的一次是什么？"

**User**: "上次让它帮我改一份活动文案的标题，结果它把整个文档都改了。我那个文档是跟客户确认过的终稿，它给我加了段落、改了语气、删了一段话。我是发出去之后才发现的。客户问我为什么内容变了，我都不知道怎么解释。"

*Signal: CRITICAL. Trust-breaking file incident with real-world consequences (client-facing document was altered). This is a severe Cautious signal that overrides all Pragmatist signals.*

> Follow-up (not needed — answer is highly specific and detailed)

### Q3 — The Relationship
> "你希望我扮演什么角色？"

**User**: "帮我干活就行，别废话。"

*Signal: executor. Reinforces Pragmatist reading. But this CONFLICTS with Q2 — they want fast execution AND they've been burned by fast execution. The override rule applies.*

### Q4 — Brake vs. Gas
> "我改你文件之前，要先问你还是直接改？"

**User**: "直接改吧，快。"

*Signal: Gas. User says Gas. But Q2 describes exactly the disaster that Gas mode produces. This is a conflict.*

**OVERRIDE RULE ACTIVATED:** Q2 trust-breaking file incident overrides Q4 Gas answer. User may SAY Gas because they're impatient, but their STORY says Brake. Generate Brake config + add hooks. In CONFIRM state, explain why: "I know you said 'just do it,' but based on what happened with that client document, I'm going to add a safety check for file changes. You can always turn it off later if it feels too slow."

### Q5 — The Handoff
> "你最想让我帮你接手什么？"

**User**: "写广告文案和活动方案的初稿。"

*Signal: content generation. Search for writing/marketing skills.*

---

## Analysis (internal)

**Surface archetype**: Pragmatist — terse answers, "快点搞", executor preference, Gas.
**True archetype**: Cautious — Q2 incident is severe (client-facing damage, post-delivery discovery).
**Resolution**: **Q2 override wins.** Trust-breaking file incident always forces Brake + hooks, regardless of other signals. The user's impatience is real, but their trauma is realer.
**Literacy**: Standard (B).

---

## Generated CLAUDE.md

```markdown
# 我的 AI 工作契约

## 怎么跟我说话
- 简短直接，不废话。
- 先给结论再说原因（如果需要原因的话）。
- 用中文。

## 我是做什么的
市场。主要用你帮我写广告文案、活动方案、看数据。

## 绝对不能做的事
- 绝对不能改我没让你改的内容。我说改标题，就只改标题，其他一个字都不要动。
- 绝对不能删我文档里的任何段落或内容。
- 绝对不能改文档的语气和风格，除非我明确让你改。
- 不要加我没要求的内容。

## 自主权
- 改文件之前必须先让我确认。我知道这样慢一点，但上次的教训太深了。
- 读文件、搜索这些不改东西的操作可以直接做。

## 交付
- 做完告诉我改了什么，简短就行。
- 如果是文案初稿，标注哪些地方是你拿不准的。
```

**Note**: Voice & Tone is Pragmatist (terse, direct), but Hard Rules and Autonomy are Cautious (strict, Brake). This is the correct resolution — respect the user's communication style while protecting against the failure mode they actually experienced.

---

## Translation Log

| Answer | → | Rule | Why |
|---|---|---|---|
| Q2: "把整个文档都改了" | → | "我说改标题，就只改标题，其他一个字都不要动" | Direct incident → maximally specific scope rule |
| Q2: "删了一段话" | → | "绝对不能删任何段落或内容" | Each violation dimension gets its own rule |
| Q2: "改了语气" | → | "绝对不能改语气和风格" | Distinct from content changes |
| Q2: "客户问我为什么内容变了" | → | Reinforces Brake + hooks. Real-world consequences = maximum safety. | The COST matters as much as the incident |
| Q3: "帮我干活就行，别废话" | → | Voice & Tone: terse, direct | Respected — conflict is in autonomy, not tone |
| Q4: "直接改吧，快" (OVERRIDDEN) | → | Brake autonomy + hooks | Q2 override rule: incident trumps stated preference |

---

## Hooks Generated

Despite user saying "直接改", hooks are added because of Q2 override:

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

**How this was explained to the user in CONFIRM state:**

> "你说了让我直接改、快一点。但你之前说的那次经历——客户终稿被改了还没发现——这个太严重了。所以我还是加了一个安全检查：每次改你文件之前，会先问你同意不同意。你觉得太慢了随时可以关掉。"

## Skills Recommended

Based on Q5 (ad copy + campaign proposals), searched `copywriting marketing`:
- **copywriting** (coreyhaines31/marketingskills) — 49.6K installs — structured marketing copy generation
