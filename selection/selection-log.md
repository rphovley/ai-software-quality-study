# Selection log

Companion to [`candidates.csv`](candidates.csv). The CSV records the verdict for
every candidate against each of the eight inclusion rules in
[methodology.md](../methodology.md) section 3.1. This file explains the calls
that the CSV cannot carry: how an ambiguous rule was read, why a borderline
candidate fell on one side of a line, and where a rule was applied to a
candidate that the rule's wording did not obviously anticipate.

Written before screening, as part of the preregistered scaffold. Entries are
appended as screening proceeds.

## How to read the CSV

| Column                | Meaning                                                                 |
| --------------------- | ----------------------------------------------------------------------- |
| `name`                | Project or subject name as its maintainers write it                     |
| `repo`                | Canonical source repository (`owner/name` on GitHub, or a full URL)     |
| `category`            | Short descriptor of what the software is (terminal, editor, CLI, …)     |
| `status`              | `included` / `excluded` / `unscreened`                                  |
| `rule_1` … `rule_8`   | `pass` / `fail` / `unknown` against the correspondingly numbered rule   |
| `evidence_links`      | Sources backing the per-rule verdicts, separated by spaces              |
| `notes`               | Why this verdict; pointer to the section below for judgment calls       |

The rules, abbreviated (authoritative text is in methodology.md section 3.1):

1. Open source
2. Client or web software (excludes deep tech and infrastructure)
3. Public writing on AI
4. Public issue tracker
5. At least 12 months of repository history as of 2026-10-31
6. Artifacts of agentic workflows in the repo (not AGENTS.md / CLAUDE.md alone)
7. Visible testing and release process
8. Popularity threshold — 75% of in-window releases draw an issue within 14 days

A candidate is included only if it passes every rule. Rule 8 is checked after
collection, so it is recorded as `unknown` during screening; a candidate that
passes rules 1–7 is marked `included` pending that check, and any later failure
on rule 8 is recorded here with the date.

## Standing interpretations

Decisions about how a rule is read, applied to every candidate alike. Recorded
here rather than in methodology.md, which is preregistered and not edited in
place.

_None recorded yet._

## Judgment calls

One entry per candidate whose verdict required a reading rather than a lookup.

_None recorded yet._

## Exclusions

Candidates that failed one or more rules, with the rule and the evidence.

_None recorded yet._

## Candidates considered and set aside before screening

Subjects that were discussed but are not screened against the rules at all,
with the reason. Recorded for completeness so the pool is reconstructable.

_None recorded yet._
