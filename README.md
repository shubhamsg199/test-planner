# Test Planner — Agentic AI Skill

An AI-powered Cursor Agent Skill that generates structured test plans from Jira tickets, feature descriptions, or PR diffs.
## Quick Start

1. Open the Robottelo workspace in Cursor
2. The skill is auto-discovered from `.cursor/skills/test-planner/`
3. Ask the agent:

```
Generate a test plan for the new Image Mode feature
```
```
What tests should we write for SAT-12345?
```

4. Review the output and refine interactively

## What It Does

The agent follows a four-step workflow:

1. **Requirement Analysis** — extracts testable requirements, maps to CaseComponent, identifies relevant interfaces (API / CLI / UI)
2. **Coverage Analysis** — searches existing tests to find what's covered and where the gaps are
3. **Test Plan Generation** — produces scenario tables with test types, importance, markers, and fixture recommendations
4. **Review** — presents the plan for feedback

It follows Robottelo conventions automatically: naming (`test_positive_*` / `test_negative_*`), docstrings (`:id:`, `:steps:`, `:expectedresults:`), fixture scoping, and markers.

## Files

| File | Purpose |
|------|---------|
| [`SKILL.md`](SKILL.md) | Skill definition — the workflow the agent follows |

## Limitations

This is a **planning accelerator**, not a test-writing replacement. It handles the mechanical work (scanning tests, building scenario tables, checking conventions) with high confidence (85-95%), but generated test code still needs human refinement for assertions and domain-specific edge cases.
