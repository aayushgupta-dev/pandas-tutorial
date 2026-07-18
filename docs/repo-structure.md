# Repo structure

How this repository is organized, and the rules for keeping it that way as it grows. This is a process doc (how the repo is laid out), not a pandas concept doc — it isn't part of the concept table in the README.

## Layout

```
pandas-tutorial/
├── main.py            # plain uv-init scaffold entry point — not lesson code, leave as-is
├── scripts/            # one file per topic that has a code component
│   └── load_data.py
├── docs/                # one markdown file per topic discussed (code or not)
│   ├── repo-structure.md   # this file
│   ├── dataframes-and-series.md   # no script — purely conceptual topic
│   └── load-data.md
├── data/                 # read-only fixture datasets
│   └── orders.csv
├── notebook.ipynb        # optional — only if working in a notebook instead of scripts/
├── pyproject.toml / uv.lock / .python-version
└── README.md
```

## Rules

**`scripts/` — one file per topic, only when the topic has a code component.**
Name files by topic, `snake_case`, e.g. `load_data.py`, `filtering.py`, `cleaning.py`. No numeric lesson prefixes (`01_...`) — order is implicit from the README's docs table, not the filename. Keep scripts near-comment-free: add a comment only if something is genuinely non-obvious. Long explanations belong in `docs/`, not in the script. Not every topic gets one — purely conceptual topics have a doc with no matching script.

**`docs/` — one file per topic discussed, code or not.**
Every topic gets a `docs/<topic-in-kebab-case>.md`, written right after the user's explanation is validated — it doesn't wait on a script existing. When a topic does get a script, the doc filename mirrors it (script's stem, underscores swapped for hyphens: `load_data.py` → `load-data.md`); when it doesn't, name the doc after the topic directly (e.g. `dataframes-and-series.md`).

These docs are **the user's own notes, in their own words** — not Claude-authored technical documentation. The flow (see CLAUDE.md's "Learning flow" section): the user explains the concept back after watching the video, Claude validates/corrects that explanation, the doc gets written from that validated explanation — simple, plain-language, "here's what this is and why," not formal reference prose. If a code component exists or gets added later, the doc links back to its script and notes the production-grade equivalent where the tutorial's version is intentionally simplified (per CLAUDE.md's "not just a tutorial-follow-along" section). If the user's understanding of a topic changes later, the doc gets updated in place rather than left stale.

**`data/` — read-only fixtures only.**
Scripts read from `data/`, never write into it. Any derived output (a cleaned/filtered/aggregated CSV, etc.) goes to a new file elsewhere, never back into `data/`. `.gitignore` enforces this: everything under `data/` is ignored except the specific fixture files explicitly un-ignored (`!data/orders.csv`). If you add a new fixture, whitelist it the same way.

**`main.py` stays inert.**
It's the placeholder `uv init` created. Lesson code lives in `scripts/`, not here — this keeps the entry point stable and avoids one file silently accumulating every lesson's code.

**`README.md` stays a high-level index.**
Setup instructions plus the docs table (topic → doc → script). Update that table whenever a new `docs/` file is added. Long-form explanation belongs in `docs/`, not README.

## Why this shape

The failure mode this avoids: a tutorial repo where every lesson's code gets appended to (or overwrites) a single `main.py`, comments pile up explaining half-remembered concepts inline, and there's no separate place for "why" vs "what." Splitting **scripts** (what/how — runnable, minimal comments) from **docs** (why — explanation, links, production notes) from **data** (fixtures, read-only) keeps each concern in exactly one place, so the repo stays navigable as more lessons get added instead of turning into an undifferentiated pile of files in the root.
