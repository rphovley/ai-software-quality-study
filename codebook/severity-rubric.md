# Severity rubric

Operationalizes the severity weights preregistered in
[methodology.md](../methodology.md) section 4.3. The three levels and their
weights are fixed by the preregistration and are reproduced verbatim below.
Everything after that is coding guidance: it tells a coder how to apply the
three levels consistently, and it does not add, remove, or reweight levels.

## The preregistered levels

> **Severity weights** (rubric in `codebook/severity-rubric.md`):
>
> - 3: crash, data loss, or security issue
> - 2: core workflow broken
> - 1: cosmetic or edge case

## Scope: what gets coded

Only **confirmed bugs** as defined in section 4.3 — issues labeled as bugs per
the project's label mapping in `config/projects.yaml`, excluding duplicates and
issues closed as "works as intended" or "not planned." Feature requests,
questions, support threads, and build-environment problems local to one
reporter are not bugs and are not coded.

Severity is coded from **the reported impact on the user**, not from the size
of the fix. A one-line fix for a crash is severity 3; a large refactor to
correct a misaligned icon is severity 1.

Severity is coded as of **the state of the report**: what the reporter and the
maintainers describe in the issue thread. If a maintainer corrects the
reporter's account ("this only happens with the beta renderer"), the correction
governs.

## Level 3 — crash, data loss, or security issue

The user loses the process, loses their work, or is exposed.

Code 3 when the report describes any of:

- the application crashing, hanging unrecoverably, panicking, or failing to
  start at all;
- loss or corruption of user data or user-visible state — unsaved buffers,
  configuration, session or window state, files written incorrectly;
- a security or privacy consequence — secrets or tokens written to logs or
  disk unprotected, credentials leaked, sandbox or permission boundary
  bypassed, remote input able to execute code.

### Examples

1. *Terminal emulator segfaults when a pane is split while a full-screen
   program is running.* Crash → **3**.
2. *Editor overwrites the file on disk with an empty buffer after an external
   change is detected.* Data loss → **3**.
3. *CLI writes the user's API key in plaintext to the debug log at default
   verbosity.* Security issue → **3**.

### Boundary with level 2

A crash confined to one non-essential surface is still a crash: code **3**.
Severity 3 is about the kind of harm, not how many users meet it. Frequency is
not part of the rubric and is not used to discount a 3.

## Level 2 — core workflow broken

The application keeps running, but something a user came to the software to do
does not work, and there is no obvious way around it.

Code 2 when the report describes any of:

- a primary function of the software producing wrong output or no output —
  for a terminal, drawing text; for an editor, editing and saving; for a CLI,
  its main subcommands; for a desktop environment, launching and switching
  windows;
- a documented feature not working as documented, on a supported platform
  and a supported configuration;
- a **regression** in behavior that previously worked, where the broken
  behavior is a primary function. (Regressions are additionally counted as
  their own metric per section 4.3; that counting is independent of severity
  and does not change the severity code.)

### Examples

1. *Shell integration stops reporting the working directory, so new panes all
   open in `$HOME`.* Documented feature, primary workflow → **2**.
2. *`--output json` emits a trailing log line, so the output no longer parses.*
   Main subcommand produces unusable output → **2**.
3. *Keyboard shortcut for switching workspaces stops responding after the
   display is re-plugged.* Primary function of a desktop environment → **2**.

### Boundary with level 1

If a documented, convenient workaround exists and the maintainers treat it as
the answer rather than as a stopgap, code **1**. If the workaround is "rebuild
from source," "downgrade," or "restart the application," the workflow is
broken: code **2**.

## Level 1 — cosmetic or edge case

The software does what the user wanted, imperfectly; or it fails only in a
configuration few users reach.

Code 1 when the report describes any of:

- visual or presentational defects that do not block the task — spacing,
  alignment, color, truncated or wrongly cased labels, a cursor drawn a pixel
  off;
- wrong or unclear text in messages, help output, or documentation;
- a failure reachable only through an unusual configuration, an unsupported
  platform, or a rare input, where ordinary use is unaffected.

### Examples

1. *Tab title keeps a stale program name for a second after the program
   exits.* Presentational, self-correcting → **1**.
2. *`--help` lists a flag that was renamed two releases ago.* Wrong text →
   **1**.
3. *Ligature rendering breaks with one particular font at non-integer display
   scaling.* Rare configuration, ordinary use unaffected → **1**.

### Boundary with level 3

A cosmetic defect that hides information the user needs to avoid harm is not
cosmetic. If a progress indicator reads "complete" while the operation failed
and the user discards the source as a result, the consequence is data loss:
code **3**.

## Tie-breaking

Applied in order, so two coders reach the same number:

1. **Highest level any single reported consequence reaches.** An issue
   describing both a misaligned border and an occasional crash is a **3**.
2. **Reported impact over hypothesized impact.** A reporter's speculation that
   a defect "could probably corrupt the database" does not make it a 3 unless
   corruption is actually reported or a maintainer confirms it.
3. **When a report spans two levels and nothing resolves it, code the lower
   level,** and list the issue in the ambiguous log below. Coding down is the
   conservative direction: it cannot inflate the severity-weighted bug counts
   that are the study's primary outcome.

## Multi-defect and split issues

One issue, one severity code — the highest, per tie-break 1. If maintainers
split a report into separate issues, each resulting issue is coded on its own
and the original is treated as a duplicate (and so excluded per section 4.3),
with the split recorded in the notes.

## Reliability

A hand-checked sample of coded issues per project, with agreement rates against
the LLM classifier used where labels are missing, is reported in
`validation/`, per section 5. Disagreements are listed with the code each coder
assigned and the tie-break that applies.

## Ambiguous codings

Issues coded under tie-break 3, recorded so they can be revisited as a
sensitivity check.

| Issue | Levels considered | Coded | Why unresolved |
| ----- | ----------------- | ----- | -------------- |
| _none yet_ | | | |
