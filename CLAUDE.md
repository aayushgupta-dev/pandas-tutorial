# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Rules

Before doing anything (commits, PRs, any GitHub-related action, or otherwise), check `.claude/rules/` for applicable rules and follow them. Currently:

- [github.md](.claude/rules/github.md) — GitHub-related rules (commit authorship, etc.)

As more rules get added under `.claude/rules/`, check the whole directory, not just the ones listed above.

## What this repo is

A learning workspace for the user to pick up **pandas** by coding along with Tech With Tim's tutorial:

- Video: https://www.youtube.com/watch?v=EXIgjIBu4EU
- Companion repo: https://github.com/techwithtim/PanadasTutorial

The video covers pandas fundamentals — DataFrames/Series, loading CSVs, exploring, indexing/filtering (including boolean masks), CRUD-style operations, cleaning data, groupby/aggregation, sorting, and saving output.

**This is not just a tutorial-follow-along.** The user explicitly wants to go past tutorial-level code to how pandas is actually used in industry. For every concept the video introduces, after showing the tutorial's version, also show (briefly) the more robust/idiomatic real-world equivalent where it differs — e.g.:

- Chained indexing (`df[df.x > 0]['y'] = ...`) → `.loc[]` to avoid `SettingWithCopyWarning`.
- Loops over rows → vectorized ops / `.apply()` only as a last resort.
- Implicit dtypes → explicit dtype checks (`df.dtypes`, `pd.to_datetime`, `category` dtype for low-cardinality columns) since silent dtype bugs are a common real bug source.
- One-off scripts → method chaining (`.pipe()`, chained `.assign()/.query()`) for readable transformation pipelines.
- No validation → sanity-checking shape/nulls after loads and merges (`df.isnull().sum()`, `assert` on expected row counts) before trusting downstream results.

Don't over-teach this — one or two lines of "here's how this looks in production" alongside the tutorial code is enough. The goal is faster, more durable learning, not a second course.

## Toolchain

- **Package manager: `uv` only.** Add deps with `uv add <pkg>`, run with `uv run <cmd>`.
- **Python: 3.12+** (pinned via `.python-version`).
- **Core dependency: `pandas`**.
- **Virtual env: `.venv/`**, uv-managed, gitignored.

Common commands:
```
uv sync                              # install/sync deps from pyproject.toml + uv.lock
uv add <pkg>                         # add a runtime dependency
uv run python scripts/<name>.py      # run a concept script
uv run jupyter lab                   # if working through notebook.ipynb instead of scripts/
```

## Repository shape

Full layout and the rules for maintaining it live in **[docs/repo-structure.md](docs/repo-structure.md) — read it before adding or moving any file.** Summary: `scripts/` holds one runnable file per concept, `docs/` holds a matching 1:1 explanation per script, `data/` holds read-only fixtures only, and `main.py` stays the inert `uv init` stub.

## Learning flow

This is a conversational, coding-along session — not "explain the topic and write the code for the user." Per topic, the loop is:

1. The user watches Tech With Tim cover a concept, then explains it back in chat, in their own words.
2. **Validate that explanation first, before anything else.** Confirm what's right, correct what's wrong and explain why. Do this even when the explanation is close to correct — the nuance is often the point.
3. The user writes the code themselves, in a new file under `scripts/` (one file per topic), mirroring what Tim does in the video. **Don't write the lesson script for them.** Your job once it exists is to review it: confirm it's correct, and — per the "not just a tutorial-follow-along" section above — point out the production-grade equivalent where relevant.
4. Once the script exists and the explanation has been validated, write the paired `docs/<topic>.md` — see [docs/repo-structure.md](docs/repo-structure.md) for how that doc should read (the user's own words, not a formal reference).
5. Update the README docs table.

If the user revisits a topic later and their understanding has shifted, update that topic's doc to reflect the new understanding rather than leaving the old version in place.

## Mock tests

After every 3-4 new topics (new `docs/*.md` files) since the last one, proactively offer a short recap quiz using the **AskUserQuestion** tool covering those topics — a handful of questions checking whether the concepts actually stuck, not just whether the code runs. Keep it quick and low-stakes, not a formal exam.

## How to assist the user effectively

- The user is learning pandas from scratch alongside the video — explain *why* a pandas idiom works the way it does (e.g. why boolean masking beats `.iterrows()`), not just *that* it works.
- Follow the video's progression; don't jump to advanced pandas (multi-indexing, custom accessors, `pd.eval`) before it's reached, unless it's a quick "in production you'd instead..." aside per the section above.
- When something in the video and current pandas API/best-practice have diverged (e.g. `inplace=True` is discouraged now, chained assignment warnings), flag it — don't silently follow outdated tutorial patterns.
- If unsure where the user is in the video, check `scripts/`/`docs/` contents and recent commits.
