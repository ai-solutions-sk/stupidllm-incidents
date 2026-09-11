# Schema

Every incident is one Markdown file, `incidents/STUPID-YYYY-NNNN.md`, with YAML
frontmatter and a prose body. This page is generated from the 70 records
currently in the corpus, so the enumerations below are what is actually in use
rather than what was once intended.

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
| `severity` | yes | 0–10, one decimal. See the rubric. |
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
| `verified` | yes | A human opened the source and confirmed the account. |
| `reproducible` | yes | Whether a reproduction exists. Most are `false`; that is honest, not a gap. |

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

Severity is assigned against the
[published rubric](https://www.stupidllm.com/methodology/), not by feel. The
rubric scores damage only: what was destroyed, leaked or lost, in four bands.

An earlier version of this paragraph said severity also weighed whether the
loss was recoverable, whether it reached production, whether the user had
authorised the action, and whether the agent misreported afterwards. It did
not, and never had. Those four are things a reader can check against a source
independently of the number, which is the argument for recording them as
fields of their own rather than folding them into the score. Which of them
belong in severity at all is an open question; the first blind re-scoring
round (`ops/inter-rater/` in the source repo) exists to find out. Corrected
2026-09-11.
