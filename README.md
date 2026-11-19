# PM Builder Team

An AI builder team for Claude: a **PM twin** that thinks like Srini, a **CTO partner**, a **UX lead**, and an **evals lead** that checks the others. A coordinating session routes work between them using a shared interaction protocol and turns a product idea into a reviewed, graded PRD.

## What's here

```
CLAUDE.md                   # imports the protocol so Claude Code loads it
.claude/agents/             # symlinks to the agent files below
ai-builder-team/
  pm-twin.md                # the what and why: strategy, PRDs, eval plans, metrics
  cto-partner.md            # the how, readiness, and cost: architecture, feasibility, security, scale
  ux-lead.md                # the user: research, flows, AI interaction patterns, copy, prototypes
  evals-lead.md             # the quality bar: rubric-based trust verdicts (gates G1–G5, rubrics R1–R8)
  interaction-protocol.md   # workflows W0–W7, handoff packets, revision limits, disagreement rules, L1/L2/L3 tiers
  project-instructions.md   # the session brief used in the claude.ai Project
examples/
  trend-ebooks-prd.md       # example output: Trend Explainer Engine PRD after two review rounds
```

## How to use it

**Source of truth:** the claude.ai Project "PM Builder Team". This repo is its version history; changes flow Project → GitHub, so edit the Project, not these files.

**In Claude Code:** clone the repo and open it. Nothing to copy:
- `.claude/agents/` holds symlinks to the four agent files, so they load as subagents.
- The root `CLAUDE.md` imports `ai-builder-team/interaction-protocol.md`.

On Windows, enable symlinks before cloning (`git config --global core.symlinks true`, with Developer Mode on).

**In a new claude.ai Project:** add the five files in `ai-builder-team/` as Project docs and paste `project-instructions.md` into the Project's instructions.

Then give it a product concept. For a new feature or PRD it runs the **W2 Feature** loop: pm-twin drafts → cto-partner and ux-lead review independently → pm-twin revises → evals-lead grades.

## Ground rules

- No fabricated evidence, metrics, users, or quotes. Unknowns are labeled.
- Prototype, beta, and GA are never blurred.
- Anything L3 (commitments, external comms, thresholds, legal, privacy) is drafted for a human to decide, never executed.
