# Prompts

Placeholder. Written as part of the preregistered scaffold; contents to follow
before classification runs.

LLM classification prompts used by the study, with pinned model versions, per
[methodology.md](../methodology.md) sections 4.2, 4.3, and 5.

## What goes here

| Prompt | Used for | Methodology |
| ------ | -------- | ----------- |
| Feature → product area classifier | Assigning each changelog feature entry to one of the project's 8–15 product areas | 4.2 |
| Bug classifier | Classifying issues as confirmed bugs where label mappings are missing | 4.3 |
| Severity classifier | Applying the 3/2/1 rubric in [`codebook/severity-rubric.md`](../codebook/severity-rubric.md) where labels are missing | 4.3 |
| Embedding model | Feature-description similarity robustness check | 4.2 |

## Rules for this directory

- Each prompt is a file with its **model ID and version pinned in the file**,
  not supplied at call time.
- A prompt is **fixed before it is run** over the data. Changes after that get
  a new file with a new version suffix; the old file stays.
- Every prompt's agreement with a hand-checked sample is reported in
  [`validation/`](../validation/), per section 5.
- Prompts are referenced by filename from `config/projects.yaml` and from the
  scripts that call them, so a result can be traced to the exact text that
  produced it.
