# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

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

## How to assist the user effectively

- The user is learning pandas from scratch alongside the video — explain *why* a pandas idiom works the way it does (e.g. why boolean masking beats `.iterrows()`), not just *that* it works.
- Follow the video's progression; don't jump to advanced pandas (multi-indexing, custom accessors, `pd.eval`) before it's reached, unless it's a quick "in production you'd instead..." aside per the section above.
- When something in the video and current pandas API/best-practice have diverged (e.g. `inplace=True` is discouraged now, chained assignment warnings), flag it — don't silently follow outdated tutorial patterns.
- Keep the `scripts/` ↔ `docs/` pairing and the README table in sync as new concepts are added — this is the main lever keeping the repo from turning into a pile of unexplained scripts as it grows.
- If unsure where the user is in the video, check `scripts/`/`docs/` contents and recent commits.
