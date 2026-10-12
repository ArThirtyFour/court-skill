# Court

A skill for adversarial comparative analysis of two options (X vs Y).

## What this is

Single-model comparisons hedge and flatter. This skill runs a structured court: advocates defend each side with evidence, independent experts verify contested facts, and independent arbiters issue the verdict.

## Skill Structure

```
court-skill/
├── SKILL.md              # Core instructions
├── README.md
└── LICENSE
```

## Installation

```bash
npx skills add ArThirtyFour/court-skill
```

Manual install — copy `SKILL.md` into your agent's skills dir:

| Agent | Project | Global |
|---|---|---|
| Claude Code | `.claude/skills/court/` | `~/.claude/skills/court/` |
| OpenCode | `.opencode/skills/court/` | `~/.config/opencode/skills/court/` |
| Cursor | `.cursor/skills/court/` | `~/.cursor/skills/court/` |
| Codex | `.codex/skills/court/` | `~/.codex/skills/court/` |
| Gemini CLI | `.gemini/skills/court/` | `~/.gemini/skills/court/` |
| Copilot | `.github/skills/court/` | `~/.copilot/skills/court/` |
| Any (`~/.agents`) | `.agents/skills/court/` | `~/.agents/skills/court/` |

## Quick start

**Claude Code / OpenCode / Cursor / Codex / Gemini:** install via command above, then ask for a `"court"` or `"X vs Y"`.

**Claude Projects:** Upload `SKILL.md` to project knowledge.

**Custom instructions:** Copy the judge workflow and verdict format from `SKILL.md`.

**API calls:** Include `SKILL.md` in your system prompt. Spawn advocates and arbiters as subagents.

## What it does

**Preparation** - Defines the dispute as "X is better than Y *for whom*", sides A/B, arbiters, materials, criteria, and audience.

**Scale (5 levels)** - Defaults to Level 1 Lite (1 for X + 1 for Y + 1 arbiter, 3 agents total). Escalates to Level 5 Maximal (20 agents) only on explicit request. Each advocate gets a unique angle so briefs do not echo each other.

**Wave strategy** - Launches subagents in strict waves (Side A, Side B, Experts, Arbiters), never all at once. Restarts failed sides on a different model. Never issues a verdict with a missing side.

**Brief format** - Advocates must output `CASE` (3-5 points, each with proof: `file:line`, URL, or data), `ONE-LINE CLOSER`, `CONCESSIONS` (2-3 honest concessions, mandatory), `CONFIDENCE` (low/med/high). No proof = no claim.

**Rules** - Truth over politeness. Evidence or silence. No cowardly ties. No flattery. Subagents must use tools (`explore`, `grep`, `read`, `websearch`) to verify claims.

## Verdict

Scorecard with weighted criteria:

| Criterion | Weight | X | Y |
|-----------|-------:|--:|--:|
| ... | ... | ... | ... |
| **Total** | 100% | **...** | **...** |

Winner with margin (`Narrow | Clear | Wide`) and confidence (`Low/Med/High`), reasoning, dissenting opinions, and flip conditions (what would change the verdict).

## Author

[AR34](https://github.com/ArThirtyFour)

## License

MIT. Use freely, share widely.
