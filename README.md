# PM Builder Team

An AI builder team that runs inside Claude Code: a **PM twin** that thinks like Srini, a **CTO partner**, a **UX lead**, and an **evals lead** that checks the others. The main Claude Code session coordinates them using a shared interaction protocol and turns a product idea into a reviewed, graded PRD.

## What's here

| Path | What it is |
|---|---|
| `CLAUDE.md` | The team interaction protocol: workflows W0–W7, handoff packets, revision limits, disagreement rules, reporting, and L1/L2/L3 authority tiers. Claude Code loads it automatically. |
| `.claude/agents/pm-twin.md` | PM twin: the *what* and *why* (strategy, PRDs, eval plans, metrics) |
| `.claude/agents/cto-partner.md` | CTO partner: the *how*, readiness, and cost (architecture, feasibility, security, scale) |
| `.claude/agents/ux-lead.md` | UX lead: research, flows, AI interaction patterns, copy, prototypes |
| `.claude/agents/evals-lead.md` | Evals lead: rubric-based trust verdicts (gates G1–G5, rubrics R1–R8) |
| `project-instructions.md` | The session brief used in the claude.ai Project |
| `work/2026-11-19-trend-ebooks/prd-v4.md` | Example output: the Trend Explainer Engine PRD after two review rounds |

## How to use it

1. Clone the repo and open it in Claude Code.
2. The four agents in `.claude/agents/` become available as subagents, and `CLAUDE.md` tells the main session how to route work between them.
3. Give it a product concept. For a new feature or PRD it runs the **W2 Feature** loop: pm-twin drafts → cto-partner and ux-lead review independently → pm-twin revises → evals-lead grades.
4. Work lands in `work/<yyyy-mm-dd>-<slug>/`, with earlier versions kept.

## Ground rules

- No fabricated evidence, metrics, users, or quotes. Unknowns are labeled.
- Prototype, beta, and GA are never blurred.
- Anything L3 (commitments, external comms, thresholds, legal, privacy) is drafted for a human to decide, never executed.
