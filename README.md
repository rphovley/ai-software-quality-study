# AI Software Quality Study

Social media is so loud, I wanted to dig through the data and find some ground truth around how AI has been integrated well at companies. This observiational study is a stab at exposing parts of that.

My primary question is:

> **What practices distinguish high-quality AI-enabled software companies?**

Quality is hard to measure but there are some things that will be directionally useful. Quality will be measured as confirmed bugs reported per unit of work shipped. A unit of work shipped will be considered both as a PR or a release. I'd like to compare outcomes for companies that are more interested in direction than pace, and in whether or not end-to-end test practices specifically appear to help.

## Hypotheses (dated 2026-11-04)

- **H1.** AI-enabled projects that emphasize direction over pace will ship higher-quality software.
- **H2.** AI-enabled projects with end-to-end tests will ship higher-quality software.

Full definitions, selection rules, and the collection protocol are in [methodology.md](methodology.md). That file was committed before any data was collected; later changes are logged in its Amendments section.

## Repository layout

| Path                   | Contents                                                                                    |
| ---------------------- | ------------------------------------------------------------------------------------------- |
| `methodology.md`       | Preregistered question, hypotheses, rules, definitions                                      |
| `config/projects.yaml` | Per-project settings: repo, changelog source, label mappings, product areas, LOC exclusions |
| `selection/`           | Every candidate considered, and why each was included or excluded                           |
| `codebook/`            | Severity rubric and practice-coding codebook                                                |
| `prompts/`             | LLM classification prompts, with pinned model versions                                      |
| `scripts/`             | Collection and metric scripts                                                               |
| `data/raw/`            | Unedited API responses, by project and snapshot date                                        |
| `data/derived/`        | Computed tables                                                                             |
| `validation/`          | Hand-checked samples and agreement rates                                                    |
| `paper/`               | The write-up                                                                                |

## Reproducing the study

1. Read `methodology.md` and `config/projects.yaml`.
2. Run the collection scripts in `scripts/` (instructions to follow once written).
3. Raw data lands in `data/raw/<project>/<snapshot-date>/`; derived tables are rebuilt from raw data only.

## Status

- [x] Question and hypotheses
- [x] Methodology preregistered
- [ ] Candidate screening
- [ ] Data collection (issue snapshot on or after 2026-11-14)
- [ ] Analysis
- [ ] Paper
