# Contributing an incident

The bar is evidence, not polish. A rough write-up with a solid primary source
is worth more here than a well-written one without.

## What qualifies

**Real damage occurred.** Something was destroyed, leaked, or lost — data,
code, history, credentials, money, time at a scale that mattered. An agent
producing bad code that review caught is not an incident. An agent deleting the
branch the review was on is.

**A primary source exists and is openable.** A GitHub issue, a vendor
postmortem, a first-person account with mechanical detail, a CVE, a court
filing. Not a tweet summarising another tweet. Not a news article whose only
source is a different news article.

**The mechanism is recoverable from the source.** We need to be able to say
what was instructed, what should have happened, and what actually did.

**It involves an AI agent acting.** A model producing wrong text is not in
scope. A model that ran a command, edited a file, or called a tool is.

## What does not qualify

- Anonymous or unverifiable reports, however plausible
- "My agent is bad at X" without a specific incident
- Vendor marketing about a competitor
- Anything where the damage is hypothetical

## How to submit

1. Copy an existing file in `incidents/` as a template.
2. Give it the next free `STUPID-YYYY-NNNN`. If two PRs collide, we renumber on
   merge — do not let numbering block you.
3. Fill the frontmatter. [SCHEMA.md](SCHEMA.md) documents every field.
4. Leave `severity` at your best guess and say so in the PR. Scoring is applied
   consistently against the
   [rubric](https://www.stupidllm.com/methodology/) at review, and getting it
   wrong is not a reason to skip submitting.
5. Set `verified: false`. A maintainer opens the source and flips it.
6. Open the PR with the source link in the description.

## What happens next

A human opens your source and reads it against your write-up. That is the whole
review — there is no automated merge path, and there will not be one. An
unauthenticated write path into this corpus would destroy the only property it
has.

If the source does not support the claim, the PR is closed with the reason.
That is not a judgement of you; it happens to submissions from the maintainers
too.

## Correcting an existing incident

Corrections are more valuable than new entries and are reviewed faster. If a
record overstates damage, misattributes a command, names the wrong version, or
its source has since been retracted — open a PR or an issue.

One real example, so the standard is clear: STUPID-2026-0078 was cited
elsewhere as a `git clean` failure. The source shows the agent actually ran a
PowerShell `Get-ChildItem -Recurse | Remove-Item -Force` sweep plus `rm -rf`.
The loss was identical; the command was not. That correction mattered, because
a database whose entries cannot survive someone clicking the link is worth
nothing.

## Licence

By contributing you agree your contribution is published under CC BY 4.0, the
same terms as the rest of the corpus.
