# Repo structure

How this repository is organized, and the rules for keeping it that way as it grows. This is a process doc (how the repo is laid out), not a pandas concept doc — it isn't part of the concept table in the README.

## Layout

```
pandas-tutorial/
├── main.py            # plain uv-init scaffold entry point — not lesson code, leave as-is
├── scripts/            # one file per pandas concept
│   └── load_data.py
├── docs/                # one markdown file per concept, 1:1 with scripts/
│   ├── repo-structure.md   # this file
│   └── load-data.md
├── data/                 # read-only fixture datasets
│   └── orders.csv
├── notebook.ipynb        # optional — only if working in a notebook instead of scripts/
├── pyproject.toml / uv.lock / .python-version
└── README.md
```

## Rules

**`scripts/` — one file per concept.**
Name files by topic, `snake_case`, e.g. `load_data.py`, `filtering.py`, `cleaning.py`. No numeric lesson prefixes (`01_...`) — order is implicit from the README's docs table, not the filename. Keep scripts near-comment-free: add a comment only if something is genuinely non-obvious. Long explanations belong in `docs/`, not in the script.

**`docs/` — strict 1:1 pairing with `scripts/`.**
Every new concept introduced in a script gets a matching `docs/<topic-in-kebab-case>.md`. Doc filename is the script's stem with underscores swapped for hyphens (`load_data.py` → `load-data.md`). Each doc explains the *why* behind the concept, links back to its script, and — for this project specifically — notes the more production-grade equivalent where the tutorial's version is intentionally simplified (see CLAUDE.md's "not just a tutorial-follow-along" section).

**`data/` — read-only fixtures only.**
Scripts read from `data/`, never write into it. Any derived output (a cleaned/filtered/aggregated CSV, etc.) goes to a new file elsewhere, never back into `data/`. `.gitignore` enforces this: everything under `data/` is ignored except the specific fixture files explicitly un-ignored (`!data/orders.csv`). If you add a new fixture, whitelist it the same way.

**`main.py` stays inert.**
It's the placeholder `uv init` created. Lesson code lives in `scripts/`, not here — this keeps the entry point stable and avoids one file silently accumulating every lesson's code.

**`README.md` stays a high-level index.**
Setup instructions plus the docs table (topic → doc → script). Update that table whenever a new `docs/` file is added. Long-form explanation belongs in `docs/`, not README.

## Why this shape

The failure mode this avoids: a tutorial repo where every lesson's code gets appended to (or overwrites) a single `main.py`, comments pile up explaining half-remembered concepts inline, and there's no separate place for "why" vs "what." Splitting **scripts** (what/how — runnable, minimal comments) from **docs** (why — explanation, links, production notes) from **data** (fixtures, read-only) keeps each concern in exactly one place, so the repo stays navigable as more lessons get added instead of turning into an undifferentiated pile of files in the root.
