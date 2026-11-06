# Scripts

Placeholder. Written as part of the preregistered scaffold; contents to follow.
No data has been collected.

Collection and metric scripts. The collection protocol they must implement is
[methodology.md](../methodology.md) section 5.

## What goes here

| Stage | Responsibility |
| ----- | -------------- |
| Collection | Pull releases, commits, PRs, and issues per project; write unedited responses to `data/raw/<project>/<snapshot-date>/` |
| Classification | Run the pinned prompts in [`prompts/`](../prompts/) over raw data |
| Metrics | Compute pace (4.1), direction and cohesion (4.2), stability (4.3) into `data/derived/` |
| Validation | Produce hand-check samples and agreement rates into `validation/` |

## Rules for this directory

- **Raw data is never edited.** Scripts read `data/raw/` and write
  `data/derived/`. Derived tables are rebuildable from raw data alone.
- **Every pull records** its timestamp, query parameters, and the commit SHA of
  the script that produced it, per section 5.
- **Issues are paged through the issue list endpoint**, not the search
  endpoint, which caps at 1,000 results.
- **LOC exclusions** (lockfiles, generated files, vendored code) come from the
  per-project patterns in `config/projects.yaml`, not from flags at the call
  site, per section 4.1.
- Scripts are **re-runnable**: a second run against the same snapshot produces
  the same derived tables.
