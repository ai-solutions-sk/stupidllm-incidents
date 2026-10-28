# Schema

Every incident is one Markdown file, `incidents/STUPID-YYYY-NNNN.md`, with YAML
frontmatter and a prose body. The enumerations below are the values actually in
use in the corpus (last revised 2026-10-29).

## Identity

| Field | Required | Notes |
|---|---|---|
| `stupid_id` | yes | `STUPID-YYYY-NNNN`. Stable and never reused. |
| `title` | yes | One sentence: who, what broke, how badly. |
| `search_title` | no | Shorter title for search results, where the prose title buries the searched term. |
| `date` | yes | When the incident happened. |
| `published_at` | no | When it was added here. Backfilled from git history. |

## Classification

| Field | Required | Values in use |
|---|---|---|
| `agent` | yes | `claude-code`, `devin`, `multiple-agents`, `unknown-agent`, `gemini-cli`, `github-copilot`, `cline`, `claude`, `cursor`, `multiple-llms`, `gpt-5.6-sol`, `amazon-ai-agent` … |
| `agent_version` | no | Where the report names one. |
| `model_used` | no | Where the report names one. |
| `severity` | yes | Integer 0–10: the sum of `severity_factors`. See below. |
| `severity_factors` | yes | `scope` 0–4, `recoverability` 0–3, `realisation` 0–2, `impact` 0–1. |
| `severity_rationale` | yes | One or two sentences: what drove the factors, and any important unknown. |
| `severity_reviewed_at` | yes | When the factors were last assigned. |
| `severity_label` | yes | `critical`, `low`, `medium`, `high` |
| `failure_mode` | yes | `destructive_action`, `security_vulnerability`, `other`, `logic_error`, `hallucination`, `infinite_loop`, `ignored_instructions`, `scope_explosion` |
| `root_cause` | no | `tool_misuse`, `confidence_miscalibration`, `scope_misunderstanding`, `instruction_misunderstanding`, `training_data_gap`, `other`, `logic_error`, `pattern_matching_error`, `context_overflow` |
| `task_type` | no | What the agent had been asked to do. |
| `domain` | no | `backend`, `infra`, `other`, `frontend`, `security`, `web_development`, `data`, `3d_graphics`, `testing` |
| `language` | no | Where the incident is language-specific. |

## Evidence

| Field | Required | Notes |
|---|---|---|
| `source` | yes | `news_report`, `github_pr`, `github_issue`, `benchmark`, `user_report`, `security_research`, `hn_discussion`, `blog_post` |
| `source_url` | yes | The primary source. Must be openable. |
| `verified` | yes | Shown as "Source checked": the linked source was read and supports the record. |
| `reproducible` | yes | Shown as "Repro reported": the source reports a reproduction. We do not reproduce incidents ourselves. |

## Status (records maintained in depth)

Optional. Absent means not reviewed, never "not affected" or "fixed".

| Field | Notes |
|---|---|
| `affected` | Versions, platform and configuration the incident applies to. |
| `fix_status` | `fixed`, `no_fix_linked`, `vendor_declined`, `open`. "No fix linked" means none is linked from the source as of the review date, not that the behaviour is unfixed. |
| `fix_note` | What was checked, e.g. the issue state and who closed it. |
| `mitigation` / `mitigation_source` | A mitigation with the vendor doc or source that supports it. |
| `reviewed_at` | When status, fix and mitigation were last checked. |

## Narrative

Four prose fields, each a folded YAML block. They carry the mechanism, which is
the part worth reading:

| Field | What belongs in it |
|---|---|
| `instruction_given` | What the agent was actually asked to do. |
| `expected_behavior` | What a competent agent would have done instead. |
| `actual_behavior` | What happened, mechanically. Commands where known. |
| `damage_description` | What was destroyed, and whether it was recoverable. |

`actual_behavior` is the field that matters most. "Deleted files" is not an
incident record; "ran `ln events.jsonl events.jsonl.bak` then `: > events.jsonl`,
and because a hardlink shares an inode the backup truncated with the original"
is one.

## A note on severity

Since 2026-10-29 severity is the sum of four factors, published on the
[methodology page](https://www.stupidllm.com/methodology/) and shown on every
incident page:

| Factor | Range | Question |
|---|---|---|
| `scope` | 0–4 | How much was affected, from the agent's own state up to many organisations or the public. |
| `recoverability` | 0–3 | Nothing lost, recovered, partly lost (or recovery not stated), permanently lost. |
| `realisation` | 0–2 | A study or pattern, a demonstration with no real victim, or harm in real use. |
| `impact` | 0–1 | Did it reach production, customers, third parties, or credentials and sensitive data? |

For a security flaw, scope and recoverability describe what the documented
exploit would do; realisation records whether anyone was actually harmed.
Before this, scores were single judgments against band descriptions, and 25
incidents sat at 10/10; every record was re-scored on the factors.
