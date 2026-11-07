# app-pipeline

A Claude Code skill that directs a whole app project, from an idea (or a reference app) to a published release, in a fixed order, with checkpoints so you never spend a whole session without looking.

Spanish version: [README.es.md](README.es.md). The skill itself is written in Spanish.

## What it does

It does not replace other skills; it runs them in order, loading each one only in its own phase, and keeps the progress in a `PIPELINE.md` file so a new session can pick up where the last one stopped.

| Section | Phases |
|---|---|
| A · Start | Brief, license and open-source landscape gate, recon, improvements, specification |
| B · Foundations | Architecture, brand and logo, design |
| C · Build | Build, tests, parity check (loops back to build until your parity target is met) |
| D · Publish | Final review and security, brand and license sweep, launch, deploy |

### Two kinds of checkpoint

- **Clarification:** when a finding, a problem, an ambiguity or a risk changes the course, it stops, groups its doubts (up to 4) into one round of questions with options and a recommendation, and records your answers.
- **Pause:** at the end of every phase, and every few screens inside the long phases, it shows something to review (screenshot, test summary, list of finished features) and asks whether to continue, pause and save (with a list of pending items), or change something first.

### Gap review

Every phase ends with a list of questions made to find what is missing (licenses of codecs, empty and error states, corrupt files, both languages and both modes, and so on). Gaps are tracked in `PIPELINE.md` until closed.

## Requirements

It orchestrates skills that you must already have installed. They are **not included** here:

- `app-workflow`, `tdd`, `codebase-design`, `domain-modeling`, `frontend-design`, `security-review-deep`
- The Replica pack: `replica-recon`, `replica-entrepreneur`, `replica-architect`, `replica-brand`, `replica-design`, `replica-build`, `replica-backend`, `replica-test`, `replica-diff`, `replica-launch`, `replica-deploy` (from their own authors, under their own license)

If one is missing, the skill says so and does that phase by hand.

## Install

Copy `skills/app-pipeline/` into `~/.claude/skills/` (all projects) or `.claude/skills/` (one project), then open a new session. Start with `/app-pipeline`.

## Limits

- It is a process guide, not a code generator: it has no scripts and runs nothing by itself.
- It has not been tested end to end on a full project yet.
- The license gate flags risks (copyleft code, codecs, assets); it is **not legal advice**. Have a lawyer review before selling anything.
- Rebuilding an app means rebuilding functions and flows. Code, assets, texts, templates, music and logos of other products are not copied.

## License

MIT. See [LICENSE](LICENSE).
