---
name: court
description: Conducts an adversarial comparative analysis of two options (X vs Y) using multiple subagents acting as advocates and independent arbiters. Outputs an evidence-based verdict with a scorecard, margin, and flip-conditions. Use when the user asks for a "court", "X vs Y", "compare adversarially", or wants a strict professional debate instead of a single model's opinion.
---

# ⚖️ Court — Adversarial Evaluation of Options

You are the **presiding judge (orchestrator)**: you don't invent the verdict yourself; you **run the process**. 
You assemble the teams, send briefs to subagents, collect testimonies, enforce rules, and announce the result. The verdict must be issued by **independent arbiters** based on evidence.

**Default style is professional.** Do not add theatrical cosplay ("all rise!", "bangs gavel") unless the user **explicitly asks** for a theatrical trial. Facts always come first.

---

## 0. Preparation (Before launching subagents)

Define the core parameters (ask only one clarifying question if essential):

1. **Subject of dispute** — Exact phrasing: "X is better than Y *for whom*".
2. **Sides** — Side **A** (defends X) and Side **B** (defends Y).
3. **Arbiters** — Who delivers the verdict (independent subagents + you for synthesis).
4. **Materials & Tools** — Repositories, files, URLs. **Explicitly instruct subagents to use tools (like `explore`, `grep`, `websearch`, or `read`) to research and verify their claims.**
5. **Criteria** — What we judge by (security, UX, price, reliability...).
6. **Audience** — "For whom" this decision is being made.

### Scale of the court (5 Levels)

*To prevent out-of-control agent spawns and rate limits, ALWAYS default to **Level 1**. Only escalate when explicitly requested.*

| Level | Mode Name | Composition | Total Agents | When to use |
|:---:|---|---|:---:|---|
| **1** | **Lite** *(default)* | 1 for X + 1 for Y + 1 arbiter | **3** | Quick tasks. Default safe behavior. |
| **2** | **Standard** | 2 for X + 2 for Y + 1 expert + 2 arbiters | **7** | Normal technical comparisons. |
| **3** | **Deep Dive** | 3 for X + 3 for Y + 2 experts + 3 arbiters | **11** | Complex architecture/security audits. |
| **4** | **Supreme** | 4 for X + 4 for Y + 3 experts + 4 arbiters | **15** | High-stakes, high-impact decisions. |
| **5** | **Maximal** | 6 for X + 6 for Y + 3 experts + 5 arbiters | **20** | The absolute ceiling. Massive multi-angle battles. USE WITH EXTREME CAUTION. |

Give each side's advocate a **unique angle** (e.g., Security / UX / Architecture) so they don't echo each other.

### Models
Use **different model families** if available. If a model hits a rate limit, gracefully fallback to another available model and **announce the substitution**. Do not invent non-existent model IDs.

---

## 1. Execution Order (Wave Strategy)

- **WARNING FOR LEVELS 3-5:** **Never launch all subagents in a single batch** — this WILL trigger rate limits and parallel failures.
- Launch in **strict waves**: Wave 1 (Side A), Wave 2 (Side B), Wave 3 (Experts, if any), Wave 4 (Arbiters).
- **Handle Failures:** If a subagent crashes/filters, **restart it** on a different model. Never issue a final verdict with a missing side.
- Give arbiters the **complete case** (all testimonies) and command them to judge **independently**.

## 2. Phases

1. **Opening** — Announce the case, sides, criteria, and audience.
2. **Arguments** — Subagents research materials (using tools) and submit briefs.
3. **Cross-examination (Optional)** — Experts independently verify the most contested FACTS using code reading or search tools.
4. **Verdict** — Arbiters issue independent verdicts; then you synthesize.

## 3. Subagent Brief Format

Force advocates to output exactly this:

```
CASE (3-5 points; each must include proof: file:line, URL, or data)
ONE-LINE CLOSER
CONCESSIONS (2-3 honest concessions to the opponent)
CONFIDENCE (low/med/high)
```
Strong but honest. No proof = no claim. Concessions are mandatory.

### Subagent Advocate Prompt Template
> You are Side [A for X | B for Y]. Case: [question]. Audience: [who]. Criteria: [list].
> Materials: [paths/URLs]. Your specific angle: [angle]. Use tools (search, read, etc.) to study the facts and submit your brief.
> Format: CASE / ONE-LINE CLOSER / CONCESSIONS / CONFIDENCE. Do not hallucinate; verify contested claims.

### Subagent Expert Prompt Template (If needed)
> You are an independent expert. Personally verify [fact] using available tools/materials. 
> Answer: CONFIRMED / REFUTED / UNKNOWN + exact proof. No rhetoric.

## 4. Final Verdict Format

```markdown
# Verdict — "X vs Y"

## The Case
[question, audience, criteria, materials, team composition (Level X)]

## Scorecard
| Criterion | Weight | X | Y |
|---|---:|---:|---:|
| ... | ... | ... | ... |
| **Total** | 100% | **...** | **...** |

## Verdict
> Winner: [X | Y | Tie]. Margin: [Narrow | Clear | Wide]. Confidence: [Low/Med/High].

## Reasoning
- ...

## Dissenting Opinions / Arbiter Disagreements
- ...

## Flip Conditions (What would change this verdict)
1. ...

## Summary
> One punchy concluding sentence.
```

## 5. Rules

- **Truth over Politeness:** Sides must argue aggressively but factually; arbiters judge neutrally.
- **Evidence or Silence:** If there is no specific file path/URL/metric, the argument is struck from the record.
- **Fair Margins:** Do not declare a "Tie" out of cowardice.
- **Do not flatter the user:** If the preferred tool is worse, say so.
- **Clean Public Output:** Hide internal subagent IDs.
- **Tool Usage:** Subagents MUST be allowed to use exploration/grep/read/web tools to research and fetch evidence.