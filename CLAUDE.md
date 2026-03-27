# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A local-first, markdown-powered multi-agent system. No code, no database — agents are defined entirely in markdown files and run by Claude Code on demand or on a cron schedule. The system is described in more detail (in Turkish) in `README.md`.

## How the System Works

The agent lifecycle follows four phases each cycle:

1. **Read Context** — journal, strategy, audience, own memory
2. **Assess State** — evaluate KPIs, determine highest-value action
3. **Execute Skill** — run one skill per cycle using decision tree in `HEARTBEAT.md`
4. **Log to Journal** — write a dated entry to `journal/`

Agents never write to `knowledge/` (static reference). They only write to `journal/`, their own `outputs/`, and their own `MEMORY.md`.

## Key Files to Know

| File | Purpose |
|------|---------|
| `AGENT_REGISTRY.md` | Master list of all agents and their status — update when creating/retiring agents |
| `CONVENTIONS.md` | Authoritative naming rules; read before creating anything |
| `NEW_AGENT_BOOTSTRAP.md` | Step-by-step guide for creating a new agent |
| `AGENT_CREATION_CHECKLIST.md` | Pre-activation checklist; verify before registering a new agent |
| `orchestrator/PRIORITIES.md` | Current top priorities for the week |
| `orchestrator/IDENTITY.md` | What the orchestrator does and does not handle |

## Creating a New Agent

1. Copy `agents/standard-agent/` to `agents/<agent-name>/` (lowercase, hyphen-separated)
2. Follow `NEW_AGENT_BOOTSTRAP.md` in order — do not skip steps
3. Define 2–4 measurable KPIs with baselines and targets in `AGENT.md`
4. Create one `.md` file per skill in `skills/`, each mapped to a specific goal
5. Verify with `AGENT_CREATION_CHECKLIST.md` before registering
6. Add the agent to `AGENT_REGISTRY.md`

Common mistakes (from `NEW_AGENT_BOOTSTRAP.md`): too many goals, skills not mapped to goals, no measurable KPI baseline, missing weekly review in `HEARTBEAT.md`.

## Agent Folder Structure

```
agents/<agent-name>/
  AGENT.md          # Mission, goals/KPIs, skill map, input/output contracts
  HEARTBEAT.md      # Schedule, daily cycle, weekly review, decision tree, escalation rules
  MEMORY.md         # Agent-local learnings — what works, patterns, signals
  RULES.md          # CAN/CANNOT boundaries, handoff rules
  skills/           # One .md per skill
  outputs/          # Agent output files (YYYY-MM-DD_agent-name_description.md)
  data/imports/     # Input data (analytics CSVs, etc.)
  scripts/          # Optional helper scripts
```

## Shared Knowledge Layer

- `knowledge/STRATEGY.md` — Current priorities and quarter goals (static; never edited by agents)
- `knowledge/AUDIENCE.md` — Audience segments, pain points, language (static)
- `knowledge/BRAND.md` — Voice, tone, values, positioning (static)
- `journal/` — Living memory; all agent observations and weekly summaries are written here as dated entries using `templates/JOURNAL_ENTRY.md`

## Running an Agent Cycle

To run an agent manually, open the agent's `HEARTBEAT.md` and follow the daily cycle instructions. The decision tree specifies which skill to execute based on current state. Always read `journal/` and agent `MEMORY.md` before executing a skill.

## Conventions

- Agent folders: `agents/<lowercase-hyphen-name>/`
- Output files: `YYYY-MM-DD_<agent-name>_<description>.md` inside the agent's `outputs/`
- Journal entries: `YYYY-MM-DD_<agent-name>_<description>.md` inside `journal/`
- Skill files: `skills/SKILL_NAME.md` (uppercase)
- All knowledge files are proposals only — agents must escalate to human for actual changes

## Escalation Rules (from standard template)

Escalate to human when:
- KPI is trending down for 2+ consecutive cycles
- Task doesn't map to any existing skill
- Strategic decision is required
- A new tool or external account is needed

Escalate to orchestrator when work overlaps with another agent.