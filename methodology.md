# Methodology

This document is the study's preregistration. It is committed before any data is collected, so the git history shows that the question, hypotheses, selection rules, and measurement definitions came before the results. Any later change is recorded in the Amendments section at the bottom, with a date and a reason, rather than edited in place.

## 1. Research question

**What practices distinguish high-quality AI-enabled software companies?**

Quality means the number of confirmed bugs reported per unit of work shipped (defined in section 4).

- First written: 2026-10-28
- Revised wording: 2026-11-05 (added "AI-enabled"; changed "web companies" to "software companies")

"Companies" is used loosely: a study subject may be a company, a team, or a single maintainer of an open-source project.

## 2. Hypotheses

Both written 2026-11-04, before data collection.

**H1.** AI-enabled projects that emphasize direction over pace will ship higher-quality software.

**H2.** AI-enabled projects with end-to-end (e2e) tests will ship higher-quality software.

Rationale for H2: unit tests written by agents risk being circular, confirming that the code does what the code does. E2e tests are more likely to check the behavior the user actually wants. They are also slower and more expensive to run, which is friction in agentic workflows that run tests often, so whether a project accepts that cost is itself a signal.

### Hypothesis-to-measurement map

| Hypothesis              | Primary measurements                          | Supporting measurements                                     |
| ----------------------- | --------------------------------------------- | ----------------------------------------------------------- |
| H1: direction over pace | Feature cohesion, continuity, alignment (4.2) | Stated emphasis in public writing (4.4); release pace (4.1) |
| H2: e2e tests           | E2e presence, CI gating, scenario count (4.4) | —                                                           |
| Outcome for both        | Severity-weighted bugs per merged PR (4.3)    | Bugs per release, regressions, time to fix (4.3)            |

Lines of code changed is recorded as a variable in its own right, to be correlated against bug rates (for example, to test whether code inflation is associated with lower quality). It is not used as a normalizing denominator.

## 3. Selection

### 3.1 Inclusion rules

A candidate is included only if it meets every rule. Every candidate considered, included or not, is recorded in `selection/candidates.csv` with the reason, and notable judgment calls are explained in `selection/selection-log.md`.

1. **Open source.** Source code is publicly available.
2. **Client or web software.** Software that people use directly, such as applications, developer tools, editors, terminals, CLIs, and desktop environments. Excludes deep tech and infrastructure (databases, compilers, kernels, cloud platforms, cryptography libraries), which carry a different quality bar. Borderline cases are explained in the selection log.
3. **Public writing on AI.** Maintainers have publicly written or spoken about their AI process and their stance on AI.
4. **Public issue tracker.** Bugs can be filed publicly, so bug counts don't depend on self-reporting.
5. **History.** At least 12 months of repository history as of the end of the study window (2026-10-31).
6. **Artifacts of agentic workflows.** PRs or commits in the repository that are signed by or attributed to agents (for example, commit trailers, bot authors, "generated with" lines). Agent instruction files such as AGENTS.md or CLAUDE.md are not sufficient on their own, since they are common regardless of how much AI a project uses.
7. **Visible testing and release process.** Tests and CI configuration are in the public repository, so testing practices can be measured.
8. **Popularity threshold.** At least 75% of releases in the study window receive one or more issues within 14 days of release. This confirms the project has enough users to surface bugs within the attribution window. (Checked after collection; a project failing it is excluded and logged.)

## 4. Measurements

### Study window

- **Releases counted:** 2026-05-01 to 2026-10-31.
- **Issue data snapshot:** taken on or after 2026-11-14, so every release has its full 14-day attribution window.
- **Changelog data:** collected from 2026-02-01, because the continuity score compares each month to the three before it.

### 4.1 Pace

- **Releases per week.** Prereleases (betas, RCs, nightlies with a version tag) count as releases if published within the study window. Each release is flagged as stable or prerelease so results can also be checked with stable releases only.
- **Release size.** Commits and lines added/removed between consecutive release tags. Lockfiles, generated files, and vendored code are excluded; exclusion patterns are listed per project in `config/projects.yaml`.
- **Merged PRs per week.**
- **Agent-attributed share.** Share of commits and merged PRs carrying agent attribution. Detection patterns are stored in the repo.

### 4.2 Direction and cohesion

**Unit of analysis:** feature entries from each project's changelog. Fixes, chores, and dependency updates are excluded.

**Product areas:** for each project, 8–15 product areas are derived from the project's own documentation (for example, its manual's table of contents or docs navigation), and recorded in `config/projects.yaml` before features are classified. Each feature entry is assigned to one area by an LLM classifier with a fixed prompt (in `prompts/`), with a hand-checked sample reported in `validation/`.

Monthly scores:

- **Cohesion:** normalized entropy of features across product areas (0 = all features in one area, 1 = spread evenly across all areas).
- **Continuity:** share of the month's features in areas touched during the prior three months.
- **Alignment:** share of the month's features that map to goals stated in the project's README or vision documents.

Features that integrate external systems (for example, adding support for another model provider) are tagged as "integration" and scores are reported both with and without them.

**Robustness check:** embedding-based similarity of feature descriptions within each month and against the prior months, with the embedding model version pinned. Disagreements with the primary scores are reported.

### 4.3 Stability

**Confirmed bugs:** issues labeled as bugs per the project's label mapping in `config/projects.yaml`, excluding duplicates and issues closed as "works as intended" or "not planned." Where labels are missing, issues are classified by an LLM with a fixed rubric and a hand-checked sample.

**Attribution to a release:** a bug counts against the version it reports, or, when no version is given, the most recent release before it was filed, if it was filed within 14 days of that release.

**Severity weights** (rubric in `codebook/severity-rubric.md`):

- 3: crash, data loss, or security issue
- 2: core workflow broken
- 1: cosmetic or edge case

**Metrics:**

- **Severity-weighted bugs per merged PR** (primary quality metric)
- Severity-weighted bugs per release
- **Regression rate** (separate metric): bugs in behavior that previously worked, per merged PR and per release
- Reopened bug rate
- Median time from bug report to fix

**Popularity:** not normalized. The popularity threshold (rule 8) ensures bugs surface within the window. Because low-severity, long-tail bugs likely scale with user count, all bug metrics are also reported counting only severity 2–3 bugs as a robustness check.

### 4.4 Practices

- **E2e tests:** presence; kinds (UI, CLI, integration against real services); approximate number of user scenarios covered; whether they run in CI on every PR; whether release jobs depend on them passing.
- **Stated emphasis:** coding of public writing and documentation for whether maintainers emphasize speed of delivery or product and users. Each coded item is recorded with a quote and a link; sources are saved to the Wayback Machine.
- Additional practices are coded per `codebook/practices.md`.

### 4.5 What counts as a pattern

A practice is reported as a pattern only if it appears in all higher-quality projects and is absent or weaker in all lower-quality ones. A practice seen in a single project is reported as an observation, not a pattern.

## 5. Collection protocol

- All raw API responses are stored unedited under `data/raw/<project>/<snapshot-date>/`.
- Every pull records its timestamp, query parameters, and the commit SHA of the script that produced it.
- Issues are collected by paging through the issue list endpoint, not search, which caps at 1,000 results.
- Repository state for practice coding (tests, CI) is recorded at a pinned commit per project.
- LLM classification prompts and model versions are pinned in `prompts/`; agreement with hand-checked samples is reported in `validation/`.

## 6. Limitations

- **Association, not causation.** The study can show that practices and quality occur together, not that one causes the other.
- **Small sample and generalizability.** Open-source projects only; closed-source software may follow different patterns.
- **Survivorship.** Only projects popular and successful enough to notice are studied.
- **Bugs and releases are proxies.** They are imperfect measures of stability and of value delivered, a long-standing problem in measuring software quality.
- **Product differences.** A terminal emulator, a Linux distribution, and a coding agent have very different failure surfaces. Omarchy in particular sits near the line between client software and infrastructure.
- **Breadth by design.** Broad products will score lower on cohesion; this is a known difficulty in measuring product direction, independent of AI.
- **Popularity.** Low-severity bug counts likely scale with user base (mitigated by the severity 2–3 robustness check).
- **Reporting conventions.** Some projects route reports through discussions before issues are opened, which can undercount bugs relative to projects that accept issues directly.
- **Researcher bias.** The researcher expected direction-focused projects to perform better. Mitigated by dating hypotheses and definitions before data collection.

## 7. Decision log

| Date       | Decision                                                                                     |
| ---------- | -------------------------------------------------------------------------------------------- |
| 2026-11-04 | H1 and H2 written                                                                            |
| 2026-11-05 | LOC recorded as a variable, not a denominator                                                |
| 2026-11-05 | Feature entries taken from changelogs                                                        |
| 2026-11-05 | Regressions reported as a separate metric                                                    |
| 2026-11-05 | 14-day bug attribution window                                                                |
| 2026-11-05 | No popularity normalization; popularity threshold plus severity 2–3 robustness check instead |
| 2026-11-05 | Prereleases count as releases if within the window, flagged for a stable-only check          |
| 2026-11-05 | Study window 2026-05-01 to 2026-10-31; issue snapshot on or after 2026-11-14                 |

## Amendments

Changes made after preregistration go here, with the date, what changed, and why.

_None yet._
