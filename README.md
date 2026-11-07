# app-pipeline

Two Claude Code skills that direct a whole app project, from an idea (or a reference app) to a published release, in a fixed order.

| Skill | Style |
|---|---|
| [`app-pipeline`](skills/app-pipeline) | With checkpoints: it stops to ask when a finding changes the course, and pauses between phases so you never spend a whole session without looking. |
| [`app-straight-pipeline`](skills/app-straight-pipeline) | Straight through: it asks nothing along the way, decides with its own judgment from what you said or hinted ("a copy just for me", "charge monthly"), delivers a finished product with a summary, and asks all its questions at the end to correct it. |

Spanish version: [README.es.md](README.es.md). The skills themselves are written in Spanish.

## app-straight-pipeline in short

- **Reads your hints first:** explicit or implicit cues become decisions, each written down with its confidence and the cost of changing it.
- **Never asks mid-way**, but never skips safety: publishing, deploying, pushing, real payments, accounts, credentials and messages on your behalf are prepared and left "pending your OK".
- **Ends with `RESUMEN-FINAL.md`:** what was built, what it includes, what could not be done and why, what was verified and what was not, then one round of questions ordered by cost of being wrong. Your answers decide what is corrected and which phases are redone.

## What app-pipeline does

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

Copy `skills/app-pipeline/` and/or `skills/app-straight-pipeline/` into `~/.claude/skills/` (all projects) or `.claude/skills/` (one project), then open a new session. Start with `/app-pipeline` or `/app-straight-pipeline`.

## Limits

- It is a process guide, not a code generator: it has no scripts and runs nothing by itself.
- It has not been tested end to end on a full project yet.
- The license gate flags risks (copyleft code, codecs, assets); it is **not legal advice**. Have a lawyer review before selling anything.
- Rebuilding an app means rebuilding functions and flows. Code, assets, texts, templates, music and logos of other products are not copied.

## License

MIT. See [LICENSE](LICENSE).
