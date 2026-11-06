# Paper outline

Placeholder. Written as part of the preregistered scaffold. No results exist
yet; the sections below are the shape of the write-up, not claims.

Working title: _What distinguishes high-quality AI-enabled software projects?_

## 1. Introduction

The question, and why bugs per unit of work shipped is the quality measure.
What is new: these projects are built with agents in the loop, publicly, so the
practices and the defects are both observable.

## 2. Hypotheses

H1 (direction over pace) and H2 (e2e tests), as preregistered on 2026-11-04,
with the reasoning for H2's expectation about circular agent-written unit tests.

## 3. Method

Selection rules and the screened pool (`selection/`). Measurements: pace,
direction and cohesion, stability, practices. The study window, the 14-day bug
attribution window, and the preregistration itself — what was fixed before data
collection and what the Amendments section records as changed after.

## 4. Results

Per-project tables: pace, cohesion/continuity/alignment, severity-weighted bugs
per merged PR. Practices coded against outcomes. Robustness checks: stable
releases only, severity 2–3 only, integration features excluded, embedding-based
similarity.

## 5. Patterns and observations

Held to section 4.5: a practice is a **pattern** only if it appears in more than
one higher-quality project and is absent or weaker in one or more lower-quality
ones. A practice seen in a single project is an **observation**. This section
keeps the two labeled apart.

## 6. Limitations

From methodology section 6: association not causation; small sample;
survivorship; bugs and releases as proxies; product differences across a
terminal, a distribution, and a coding agent; breadth penalizing cohesion;
popularity effects on low-severity counts; reporting conventions; researcher
bias.

## 7. Appendices

Candidate screening table. Severity rubric and practice codebook. Prompts with
pinned model versions. Validation samples and agreement rates. Links to raw
data snapshots.
