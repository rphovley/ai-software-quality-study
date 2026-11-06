# AI Software Quality Study

An observational study of open-source, AI-enabled software projects, asking:

> **What practices distinguish high-quality AI-enabled software companies?**

Quality is measured as confirmed bugs reported per unit of work shipped. The study compares projects that emphasize direction and deliberate pace against projects that emphasize speed of delivery.

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
