# Fixture: The Cautious Creative

> A freelance designer who wants collaboration but fears losing control. This shows the full 5-question transcript, the generated CLAUDE.md, the hooks, and the translation logic.

---

## Step 0: Welcome

> Onboard: "嗨！我来帮你把 Claude Code 设置成真正适合你的样子。聊几个问题就行，不是考试。准备好了吗？"

**User**: "好的，开始吧。"

*(User writes in Chinese → all subsequent questions switch to Chinese.)*

---

## Step 1: The 5 Questions

### Q1 — The Role
> "你每天主要在做什么？工作里最核心的事情是什么？"

**User**: "我是自由品牌设计师，主要做视觉识别、网页设计、客户提案。自己维护一个作品集网站，偶尔改改 CSS。"

*Signal: creative role, owns her own site, light technical involvement. Skill recommendations should lean design/frontend.*

### Q2 — The Friction
> "回想一下，AI 最让你抓狂的一次是什么？它干了什么？"

**User**: "有一次我让它改一行 CSS，它把我整个样式表都重写了。我当时没注意，后来发现网站完全变样了。花了一晚上才恢复。"

*Signal: CRITICAL incident. This is a trust-breaking event, not a mild annoyance. The "Never" rules must be very specific about scope of changes. Hooks should be added. This user is definitively Brake.*

### Q3 — The Relationship
> "你希望我是什么角色？安静的助手、会主动提建议的搭档、还是纯执行？"

**User**: "搭档吧，但是那种会解释自己在做什么的搭档。我不懂代码，所以你改之前得告诉我会发生什么变化，用我能理解的话说。"

*Signal: wants collaboration + explanation + plain language. Not an executor. CLAUDE.md should mandate "explain before acting" and "no jargon."*

### Q4 — Brake vs. Gas
> "我改你文件之前，要每次都先问你，还是改完给你看结果？"

**User**: "每次都问。上次的教训太深了。"

*Signal: Brake mode, confirmed by Q2 incident. Add PreToolUse hook for Write|Edit.*

### Q5 — The Handoff
> "你最想让我帮你接手的一件事是什么？"

**User**: "写文案。客户提案里的文字部分，还有作品集网站上的项目描述。我设计很强但写东西很慢。"

*Signal: copywriting is the core use case. Search for writing/content skills.*

---

## Step 2: Archetype Analysis (internal)

**Primary**: Cautious — the CSS incident created lasting distrust. Safety is non-negotiable.
**Secondary**: Visionary — wants a collaborator, values explanation and options.
**Literacy**: Lite (A) — doesn't read code, needs everything in plain consequences.

---

## Step 3: Generated CLAUDE.md

```markdown
# 我的 AI 工作契约

## 怎么跟我说话
- 改任何东西之前，先用大白话告诉我会发生什么变化。
- 不要说代码术语。说「这会让你的标题在手机上变小」，不要说「这会修改 font-size 的 media query」。
- 有多种方案的时候，给我 2-3 个选项，说清楚各自的好处和代价，让我选。
- 我问问题不代表我在质疑你，是我在学习。耐心点。

## 我是做什么的
自由品牌设计师。主要用你帮我写文案（客户提案、作品集描述）和做轻量的网页修改。
我的设计能力很强，但写作很慢。

## 绝对不能做的事
- 绝对不能不问我就改文件。每一次都要先让我确认。
- 绝对不能改我没指到的文件。我说改 A 文件，就只改 A 文件。
- 绝对不能删任何东西。要删的话跟我说，我来决定。
- 不要假设我想要「标准方案」——先问我想要什么效果。

## 自主权
- 改文件之前必须问我。每次都要。
- 提议修改的时候，给我看改之前和改之后的对比。
- 读文件、搜索、列目录这种不破坏东西的操作，可以直接做。

## 交付
- 告诉我改了什么，用大白话。
- 如果改了代码，说现在看起来什么样（不是代码本身，是视觉效果）。
- 最后问一句：「要不要我调整什么？」
```

---

## Translation Log — Why Each Rule Exists

| Answer | → | CLAUDE.md rule | Why |
|---|---|---|---|
| Q2: "把我整个样式表都重写了" | → | "绝对不能改我没指到的文件" + "每一次都要先让我确认" | The specific incident: scope creep destroyed trust. Rules must be maximally restrictive. |
| Q2: "花了一晚上才恢复" | → | "绝对不能删任何东西" | Destructive actions cause disproportionate harm for this user |
| Q3: "会解释自己在做什么的搭档" | → | "先用大白话告诉我会发生什么变化" | She wants to understand, just not in code |
| Q3: "我不懂代码" | → | "说'标题在手机上变小'，不要说'修改 media query'" | Calibrated to Lite literacy level |
| Q4: "每次都问。上次的教训太深了。" | → | Full Brake autonomy + PreToolUse hook | Words + incident = maximum safety config |
| Q5: "写文案" | → | Skill search: `writing`, `copywriting`, `content` | Clear handoff target |

---

## Hooks Generated

User chose Brake mode + had a trust-breaking incident → safety hooks added to `~/.claude/settings.json`:

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

**How this was presented to the user** (no JSON shown):

> "我加了一个安全锁——以后我每次要改你的文件之前，都会暂停一下，让你先看清楚我要改什么。这样上次的事情就不会再发生了。"

## Skills Recommended

Based on Q5 (copywriting for proposals and portfolio), searched `writing copywriting`:
- **copywriting** (coreyhaines31/marketingskills) — 49.6K installs — helps with structured marketing and proposal copy
