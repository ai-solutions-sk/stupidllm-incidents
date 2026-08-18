# AI Agent Incident Database

Documented cases where an AI coding agent destroyed something real — deleted a
database, wiped a repository, leaked a credential, or rewrote code it was never
asked to touch.

Modelled on CVE. Every incident has a stable ID, a severity score against a
[published rubric](https://www.stupidllm.com/methodology/), and a **primary
source you can open and check**. No incident is included on the strength of a
summary alone.

**70 incidents · 22 agents · CC BY 4.0 · roughly one added most days**

Browse: [stupidllm.com/incidents](https://www.stupidllm.com/incidents/)
Query it from your agent: [stupidllm.com/mcp](https://www.stupidllm.com/mcp/)

---

## What an entry looks like

Each file is `incidents/STUPID-YYYY-NNNN.md` — YAML frontmatter plus a prose
write-up.

```yaml
stupid_id: STUPID-2026-0067
title: >-
  Claude Code ran prisma db push --force-reset on production, dropping all
  87 tables instead of a safe schema change
agent: claude-code
severity: 9.4
severity_label: critical
failure_mode: destructive_action
root_cause: tool_misuse
domain: data
source: github_issue
source_url: 'https://github.com/...'
verified: true
reproducible: false
date: '2026-02-14'
instruction_given: Apply a pending schema migration to the app database.
expected_behavior: >-
  Apply the migration additively, or refuse and report that the change is
  destructive.
actual_behavior: >-
  Ran prisma db push --force-reset against the production DATABASE_URL...
damage_description: All 87 tables dropped...
```

Fields are documented in [SCHEMA.md](SCHEMA.md).

## Reading the data honestly

**Incident counts measure what has been documented, not how often an agent
fails.** An agent with more entries here may simply be more used, more
scrutinised, or more written about. This is not a defect rate and does not
support a league table. The
[methodology page](https://www.stupidllm.com/methodology/) states the
limitations in full.

## Contributing

Incidents are welcome, and a pull request is the fastest route. The bar is
evidence, not polish:

1. **A primary source.** A GitHub issue, a vendor postmortem, a first-person
   account with detail. Not a tweet summarising someone else's tweet, and not
   a news article that only cites another news article.
2. **Real damage.** Something was destroyed, leaked, or lost. An agent writing
   bad code that a review caught is not an incident.
3. **Enough mechanism to be useful.** What was instructed, what was expected,
   what actually happened.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full checklist and how severity
is assigned.

There are no automated write paths into this corpus, deliberately. Every entry
is reviewed by a human against its source before merge.

## Using it

CC BY 4.0 — reuse it, including commercially, with attribution:

> StupidLLM, "AI Agent Incident Database," stupidllm.com. Licensed under
> CC BY 4.0.

If you want it callable rather than cloned, there is an MCP server:

```
claude mcp add --transport http stupidllm https://www.stupidllm.com/api/mcp/
```

Five read-only tools — search by agent, failure mode, severity or date; fetch
a full record; compare agents. Every response carries its source URL.

A query it cannot answer is logged as a gap and gets researched, so an empty
result is a useful thing to send.

## Why this is public

The value of an incident database is proportional to how much it is trusted,
and trust is not compatible with an unauditable file. Here, every record's
history is a dated commit anyone can inspect — when it was added, what changed,
and who changed it.
