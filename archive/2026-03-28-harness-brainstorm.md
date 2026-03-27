# Harness Brainstorm: Superpowers + GSD + PUA 三合一设计

## 起因
删掉 CCG 后，想把 Superpowers、GSD、PUA 三套工具组合成一个 harness。

## 关键认知

### 三套工具各管什么
- **Superpowers** = 交通警察（skill 路由器，确保 AI 用已装的 skill）
- **GSD** = 施工队 + 项目经理（plan→execute→verify 全生命周期）
- **PUA** = 精神鞭子 / 急救包（AI 卡住时逼它换方案、往深处想）

### 为什么不能简单叠加
- 上下文爆炸：三套同时加载 token 烧太快
- 权力冲突：GSD 说按 plan 走，PUA 说换方案，Superpowers 说先查 skill
- 简单任务开销太大
- PUA 全程开着变成背景噪音

### 真正的设计：发散→沉淀→执行
不是静态分级，而是动态升级。因为我的模式是「想法越聊越大」：

1. **Phase 1 自由发散** — 我主导，AI 跟随，用大白话，内置 PM 思维（帮我分辨几件事、找哇塞点、砍 scope）
2. **Phase 1.5 补全画面** — 参照物、场景故事、意外情况、反馈预设
3. **Phase 2 Scope 沉淀** — 说「开干」时一页纸确认：聊了什么、看到什么、哇塞点、大小、MVP 第一步
4. **Phase 3 执行路由** — 小事直接干，大事 GSD 只 plan MVP 第一步
5. **PUA 是急救包** — 卡住时触发，不是日常模式

### CLAUDE.md 分层哲学
- `~/.claude/CLAUDE.md` = 我这个人跟 AI 的契约（可复制去任何 CLI）
- `~/CLAUDE.md` = Claude Code 在这台机器上怎么干活
- 子项目 `CLAUDE.md` = 具体项目上下文

## 关于我的核心发现
- 代码水平 0，听不懂就直接批准走走看
- 想法驱动，享受发散，我主导方向
- 永远只想做 MVP，但要能看到可行的哇塞结果
- 身份认同在品味和眼光，不在技术
- 第一性原理思考者，想要能对拆的搭档，不是听话的执行者
- 开源对话 > 开源代码
- Top frustrations：乱存乱改、造轮子、编造、yapping
- PUA 的真正价值不是逼干活，是逼 AI 往底层逻辑冲

## 改动清单
- 删除 CCG 全部文件（rules/skills/commands/config）
- 安装 superpowers skill
- 重写 `~/CLAUDE.md` workflow 部分（三阶段 + PM 思维 + 补全画面）
- 重写 `~/.claude/CLAUDE.md`（去掉 Claude Code 专属内容，通用化）
- Claude Code 操作规范补回 `~/CLAUDE.md`
- 创建 `~/discussions/` 目录
- 创建 `~/MY_AI_CONTRACT.md` 软链接
- 多条 memory 存入
