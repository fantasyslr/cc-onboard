# Cross-AI Review — cc-onboard

Date: 2026-03-29
Reviewers: Gemini 2.5 Pro, Claude Sonnet 4.6
Codex o3: skipped (does not support stdin pipe)

---

## Scores

| Reviewer | Concept | Execution | Ship-readiness |
|----------|:---:|:---:|:---:|
| Gemini 2.5 Pro | 10 | 9 | 8 |
| Claude Sonnet 4.6 | 8 | 6 | 4 |

---

## Critical Finding (Claude Sonnet)

**The safety hook is fake.**

Current hook implementation:
```json
"command": "echo '⚠️ About to modify a file...'",
"timeout": 2
```

This exits 0 → does NOT block Claude from proceeding. It prints a warning in the terminal, then Claude modifies the file anyway. The Cautious user is told "I added a safety lock" but the lock doesn't actually stop anything.

**Impact**: This directly violates the core promise to the highest-trust-need user type.
**Fix**: Either use exit 1 to actually block, implement a real confirmation mechanism, or rename it from "safety lock" to "reminder" and set expectations correctly.

---

## Consensus Findings (both reviewers agree)

1. **Test harness needed** — No automated way to verify generated CLAUDE.md is consistent across runs.
2. **Conflict resolution rule missing** — When signals conflict (e.g., Pragmatist vocabulary + Cautious incident), no explicit priority rule exists.
3. **Fixtures need more variety** — Three out of four fixtures recommend the same skill (copywriting). No Visionary archetype fixture. No conflicting-signal fixture.

---

## Gemini 2.5 Pro — Key Points

**Strengths identified:**
- Translation Protocol (Section 13) called "the core of the skill's intelligence"
- State machine praised for clean single-responsibility per state
- Follow-up limit (1 per question) praised as "critical, pragmatic constraint"
- Failure modes handling called "impressively risk-aware"

**Improvements suggested:**
1. Add a model-suppression meta-rule in ANALYZE: "Suppress all training defaults — no filler, no apologies, no disclaimers"
2. Add explicit "safety-first" conflict resolution: "If signals conflict, always default to Cautious"
3. Review Mode should detect broken CLAUDE.md (>50 lines, contradictory, generic) and offer re-onboarding

**Single biggest weakness (Gemini):** Reliance on non-deterministic LLM to perfectly follow complex instructions.

---

## Claude Sonnet 4.6 — Key Points

**Strengths identified:**
- Translation Protocol strongest section
- "Every rule must trace to an answer" praised as critical guardrail
- 5-question structure user-friendly for target audience

**Critical issues:**
1. **Hook is fake** (see above) — P0
2. **CONFIRM state underspecified** — says "show drafted CLAUDE.md" but non-technical users shouldn't see raw markdown. Should specify: explain each section in plain language, not paste the file.
3. **README/SKILL.md mismatch** — README mentions "三档深度" (Quick/Balanced/Full) but SKILL.md doesn't implement it at all. Either implement or remove from README.
4. **Section 9a vs Section 13 conflict** — Hook rules say "Cautious archetype AND Brake", but Translation Rules say "Q2 file-destruction → force Brake + hooks". If user is Pragmatist but has Q2 incident, which wins?
5. **Character threshold doesn't work for Chinese** — "< 15 characters" as vague-answer detection fails because Chinese is much more compact ("销售" = 2 chars but complete answer).

**Other issues:**
- No `settings.json` backup before modifying (only CLAUDE.md has backup)
- Review Mode has no backup step before changes
- No Visionary archetype fixture
- `npx skills find` result quality unverified in fixtures (same skill recommended 3 times)

**Single biggest weakness (Sonnet):** "Hook is fake, but you tell the user it's real. This is a trust problem, not a bug."

---

## Prioritized Action Items

| Priority | Issue | Source |
|:---:|---|---|
| **P0** | Fix hook — must actually block, or rename to "reminder" | Sonnet |
| **P1** | Add conflict resolution rule (safety-first when signals conflict) | Both |
| **P1** | Fix CONFIRM state — explain in plain language, not raw markdown | Sonnet |
| **P1** | Resolve README/SKILL.md mismatch on "三档深度" | Sonnet |
| **P1** | Fix Section 9a vs 13 hook rule conflict | Sonnet |
| **P2** | Fix character threshold for Chinese (use info density, not length) | Sonnet |
| **P2** | Add settings.json backup before modification | Sonnet |
| **P2** | Add Visionary archetype + conflicting-signal fixtures | Both |
| **P2** | Add model-suppression meta-rule | Gemini |
| **P3** | Build automated test harness for output stability | Both |
| **P3** | Review Mode: detect broken CLAUDE.md and offer re-onboarding | Gemini |
