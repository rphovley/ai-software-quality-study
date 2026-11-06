# Practice coding codebook

Operationalizes the practice measurements preregistered in
[methodology.md](../methodology.md) section 4.4. The measurements themselves are
fixed by the preregistration; this file defines the codes, the evidence each
code requires, and where it is recorded. Additions to this codebook are
additions of *practices to code*, which section 4.4 explicitly provides for
("Additional practices are coded per `codebook/practices.md`"); they are not
changes to the hypotheses, the rules, or the outcome measure.

## Evidence standard

Every coded item carries **a quote and a link**. This is not optional and is
not satisfied by a summary in the coder's words.

| Field | Requirement |
| ----- | ----------- |
| Quote | Verbatim, from the source. Trim with `…` but never paraphrase inside quotation marks. Long enough to carry the claim without the surrounding page (roughly one to three sentences). |
| Link | Direct URL to the specific page, post, video timestamp, file, or line range — not a site root. Repository evidence is linked at a **pinned commit SHA**, per section 5, never at a branch name. |
| Archive | Public writing is saved to the Wayback Machine and the archive URL recorded alongside the live one, per section 4.4. |
| Date | Publication date of the source. Items outside the study window are coded and kept, flagged `out_of_window`. |

Spoken sources (podcasts, conference talks, streams) are quoted by transcription
with a timestamped link. The transcription is the coder's, so it is marked
`transcribed: yes` and the audio remains the authority if challenged.

A coded item with no quotable source is not a coded item. It may be recorded as
a lead in the project's notes, but it does not enter the analysis.

## Where coded items live

- Per-item rows: `data/derived/practices.csv`, one row per coded item.
- Pinned repository state for each project: recorded in `config/projects.yaml`.
- Hand-checked samples and coder agreement: `validation/`.

Row schema for `data/derived/practices.csv`:

```
project, practice, code, value, quote, source_url, archive_url, source_date,
source_type, pinned_sha, transcribed, out_of_window, coder, coded_date, notes
```

---

## Practice 1 — End-to-end tests (H2)

Section 4.4 fixes five sub-measurements. Each is coded from the repository at
the project's pinned commit, and each carries a link to the file or workflow
that justifies it.

### 1a. Presence

| Code | Meaning |
| ---- | ------- |
| `yes` | The repository contains tests that drive the software as a user does — through its real interface, with the real binary or a real build — and assert on what the user observes. |
| `no` | No such tests. |

**What counts.** The test exercises the assembled product through its outer
interface: spawning the built binary, driving the UI, issuing CLI invocations
and asserting on output, or running the app against a real dependency. Internal
function calls with mocked collaborators are unit or integration tests and are
coded `no` here, however broad their coverage.

**Borderline guidance.** Tests that boot the whole application but then assert
on internal state rather than user-observable output are coded `yes` with
`notes: asserts internal state`. The distinction is recorded rather than
resolved, so it can be checked as a sensitivity analysis.

### 1b. Kinds

Multi-code; record each kind present as its own row. Section 4.4 names three:

| Code | Meaning | Typical evidence |
| ---- | ------- | ---------------- |
| `ui` | Drives a graphical or terminal interface and asserts on what is rendered | Screenshot or snapshot comparison, synthetic input events, pty-driven terminal assertions |
| `cli` | Invokes the built command-line interface and asserts on stdout/stderr/exit status | Golden-file or snapshot CLI tests, shell-driven test scripts |
| `integration_real` | Runs against a real external service or real OS facility rather than a stub | Tests requiring API credentials, a live model provider, a real display server or window manager |

### 1c. Scenario count

An **approximate** count of distinct user scenarios covered, per section 4.4.
Approximate is the preregistered word and the coding respects it: report a
bucket, not a precise number, because test organization differs too much
between projects for a count to be comparable.

| Code | Meaning |
| ---- | ------- |
| `none` | 0 |
| `few` | roughly 1–10 distinct user-visible scenarios |
| `some` | roughly 11–50 |
| `many` | roughly 50+ |

Count **distinct user-visible scenarios**, not test functions or assertions. A
parameterized test running one scenario across forty terminal sizes is one
scenario. The counting method used for each project is recorded in `notes`, and
the files counted are linked, so the bucket can be re-derived.

### 1d. Runs in CI on every PR

| Code | Meaning |
| ---- | ------- |
| `yes` | A CI workflow triggered by pull requests runs the e2e tests, and a failure marks the run failed. |
| `partial` | Runs on PRs but is advisory — `continue-on-error`, a non-required check, or a subset of the suite. |
| `no` | Runs only on a schedule, on a branch push, or manually. |
| `n/a` | No e2e tests to run (1a is `no`). |

Evidence is the workflow file at the pinned SHA, with the trigger and the step
quoted. Where branch protection, not the workflow, is what makes the check
required, that is noted — protection settings are not always publicly visible,
and when they are not, the code is based on the workflow alone and `notes`
records the limit.

### 1e. Release jobs depend on e2e passing

| Code | Meaning |
| ---- | ------- |
| `yes` | The release or publish job will not run unless the e2e job succeeds (job-level `needs`, a gating workflow, or a documented manual gate). |
| `no` | Release can proceed independently of the e2e result. |
| `n/a` | No e2e tests (1a is `no`). |

---

## Practice 2 — Stated emphasis: speed vs product and users (H1)

Section 4.4: "coding of public writing and documentation for whether maintainers
emphasize speed of delivery or product and users."

### What is sampled

Public, attributable statements by a project's maintainers about **how they
decide what to build and how fast to ship it**:

- project README, vision, roadmap, release notes, and contribution guidance;
- blog posts, newsletters, mailing-list and forum posts by maintainers;
- recorded talks, podcast appearances, and streams;
- substantive maintainer comments in issues, PRs, and discussions where they
  explain a decision to ship, hold, or refuse something.

Not sampled: statements by non-maintainers; marketing copy neither written nor
endorsed by a maintainer; social-media posts that assert a preference with no
reasoning attached (they are too short to code reliably, and their volume
varies by how much each maintainer posts).

### Item-level codes

One row per statement.

| Code | Meaning | Quote markers |
| ---- | ------- | ------------- |
| `speed` | Emphasizes pace, volume, throughput, or shipping as the goal | shipping fast, velocity, how much we got out, daily releases as an achievement, "just ship it" |
| `product` | Emphasizes what users get, coherence, restraint, or getting it right | what the user needs, saying no, holding a release, fitting the whole, polish, "this isn't ready" |
| `both` | Asserts both, with neither subordinated | "fast *and* right", speed framed as serving the product |
| `neither` | About AI or process but not about this trade-off | which tools they use, cost, prompt technique |

`both` is a real code, not a fallback. It is used when a statement genuinely
holds the two together. When a statement mentions one in passing while arguing
for the other, code the one being argued for and quote enough to show it.

### Tension items

A statement describing a case where the two **conflicted** and the maintainer
chose is the strongest evidence for H1, and is flagged `tension: yes`. "We held
the release two weeks because the migration wasn't right" outweighs a dozen
value statements. Tension items are reported separately from the overall
balance.

### Project-level aggregation

*Operational guidance, not a preregistered measurement.* Section 4.4 fixes
item-level coding with quotes; the roll-up below exists so the coded items can
be compared across projects, and both the items and the roll-up are reported so
a reader can disagree with the roll-up and still use the items.

For each project report:

1. **Item counts** by code (`speed`, `product`, `both`, `neither`).
2. **Balance** = (`product` − `speed`) / (`product` + `speed` + `both`),
   over in-window items only, excluding `neither`. Range −1 to +1; positive is
   product-leaning. Reported with the denominator, since a balance over five
   items is not a balance over fifty.
3. **Tension items** listed in full, with quotes.
4. **Sampling note:** what was searched, how many sources were reviewed, and
   what the coder could not find. A project with little public writing yields a
   thin code, and that thinness is reported rather than smoothed over.

Per section 4.5, a difference in stated emphasis is reported as a pattern only
if it holds across all higher-quality projects and is absent or weaker in all
lower-quality ones. Otherwise it is an observation.

---

## Additional practices

Section 4.4 provides for additional practices to be coded here. Each is added
with the date it was added and the reason, so the codebook's own history shows
what was decided before data collection and what after. A practice added after
collection begins is reported as exploratory.

| Date added | Practice | Codes | Reason | Added before collection? |
| ---------- | -------- | ----- | ------ | ------------------------ |
| _none yet_ | | | | |

---

## Coder procedure

1. Pin the repository commit and record it in `config/projects.yaml`.
2. Code practice 1 from the pinned tree; link every file and workflow cited.
3. Search for practice 2 sources; log the searches in the sampling note,
   including those that returned nothing.
4. Archive each public source to the Wayback Machine; record both URLs.
5. Write one row per item to `data/derived/practices.csv`.
6. Code a second project before revisiting the first, so a project's codes are
   not calibrated against its own outcome.

**Blinding.** The coder does not look at a project's bug metrics before coding
its practices. The researcher expected direction-focused projects to perform
better (section 6), and practice coding is the step where that expectation
could most easily shape the result. Where blinding is impossible — the
researcher is coding alone and has already seen the data — that is recorded in
the project's `notes` and in the limitations.
